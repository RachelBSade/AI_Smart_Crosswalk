# SafeCross – AI Service

Python service that analyses video from a crosswalk camera, detects dangerous pedestrian behaviour and sends alerts to the backend.

## Pipeline

```
video ─> frame (1 of every N) ─> detector ─> tracker ─> risk rules ─> alert sender ─> POST /api/alerts
                                 YOLOv8      stable ID   17 cases      background queue
                                             per person  Low/Med/High  + x-api-key
```

## Files

| File | Purpose |
|---|---|
| `main.py` | Command-line entry point for video analysis |
| `video_processor.py` | The main loop: reads frames and runs the pipeline |
| `detector.py` | YOLOv8 wrapper: persons, vehicles, bicycles, motorcycles, phones |
| `tracker.py` | Gives each person a stable ID across frames (built-in tracker or ByteTrack) |
| `risk_rules.py` | Computes distance, speed and direction for each person and classifies the behaviour into one of 17 risk cases |
| `alert_sender.py` | Sends alerts to the backend on a background thread; retries once without the image |
| `config.py` | Every tunable value: model, thresholds, crosswalk zone, backend address |
| `calibrate.py` | Computes the adult height reference for a camera, used to tell children from adults |
| `verify_setup.py` | Checks that YOLO and OpenCV are installed correctly |
| `app.py` | FastAPI server: `GET /` health check and `POST /detect` for a single image |
| [`tests/`](tests) | Unit tests |

## Setup

```bash
python -m venv venv
venv\Scripts\activate            # macOS / Linux: source venv/bin/activate
pip install -r requirements.txt
python verify_setup.py --no-window
```

The YOLO weights file (`yolov8n.pt`) is downloaded automatically on the first run if it is missing.

## Analyse a video

```bash
python main.py --source path/to/video.mp4            # analyse and send alerts to the backend
python main.py --source path/to/video.mp4 --no-api   # analyse only, print the results
python main.py --source path/to/video.mp4 --show     # also open a window with boxes and the crosswalk zone
python main.py --source 0                            # webcam
```

| Flag | Meaning |
|---|---|
| `--every N` | Analyse 1 of every N frames |
| `--crosswalk`, `--camera` | IDs stored on the alerts (the `_id` values from MongoDB) |
| `--tracker simple\|yolo` | Built-in tracker, or ByteTrack |
| `--show` | Open a preview window |
| `--no-api` | Do not send alerts |

The backend must be running for alerts to be saved, and `SENSOR_API_KEY` must match the backend's value. The key is read from the environment, or from `backend/.env`.

## Run the detection server

```bash
uvicorn app:app --port 8000
```

## How the rules work

- Distances are measured in **body heights (h)** and speeds in **h/s**, so the same thresholds work at any resolution and any distance from the camera.
- The crosswalk zone is a polygon in normalized coordinates (`ROI_POLYGON_NORM` in `config.py`). It must be adjusted for each camera.
- When several rules match one person, a priority list picks the most severe.
- Low cases are only logged. Medium and High cases also set `ledTriggered`.

## Tests

```bash
python -m unittest discover -s tests -t . -v
```

The tests do not need YOLO or a GPU.
