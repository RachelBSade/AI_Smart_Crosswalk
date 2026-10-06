# AI service tests

Unit tests for the video analysis logic. They use synthetic tracks and generated video, so they need no YOLO model, GPU or backend.

| File | What it checks |
|---|---|
| `test_risk_rules.py` | Each risk case and the priority between cases |
| `test_tracker.py` | Stable IDs across frames, matching limits, phone assignment |
| `test_video_processor.py` | The full loop on a synthetic video, including when alerts are sent |
| `helpers.py` | Shared builders for fake tracks and frames |
| `test_high_alert.py` | Manual script that posts one simulated High alert to a running backend. Set the image path and API key before use |

Run from the `ai-service` folder:

```bash
python -m unittest discover -s tests -t . -v
```
