# services

Business logic. Routes call these functions, and the functions use the Mongoose models. Keeping the logic here makes the routes short and the logic easy to test.

| File | Purpose |
|---|---|
| `userService.js` | Login, password hashing, token creation and user management |
| `crosswalkService.js`, `cameraService.js`, `ledService.js` | Create, read, update and delete equipment |
| `alertService.js` | Creates alerts (uploads the snapshot first) and updates them; sets `resolvedAt` when an alert is handled |
| `analyticsService.js` | Computes the manager dashboard statistics in one MongoDB aggregation; counts only handled alerts |
| `cloudinaryService.js` | Uploads a base64 image and returns its URL |
| `detectService.js` | Sends an image to the Python AI service and returns the detections |
| `dangerAnalyzer.js` | Early rule skeleton for single-image analysis; the active risk rules run in the AI service |
