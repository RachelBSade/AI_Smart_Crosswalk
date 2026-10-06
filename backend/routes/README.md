# routes

Express routers. Each file defines the HTTP endpoints of one resource and the permissions for each endpoint. The logic itself lives in [`../services`](../services).

| File | Base path | Notes |
|---|---|---|
| `userRoutes.js` | `/api/users` | Login (public) and user management (Admin) |
| `crosswalkRoutes.js` | `/api/crosswalks` | Junction CRUD |
| `cameraRoutes.js` | `/api/cameras` | Camera CRUD |
| `ledRoutes.js` | `/api/leds` | LED strip CRUD |
| `alertRoutes.js` | `/api/alerts` | Created by the AI service; listed and handled by users |
| `analyticsRoutes.js` | `/api/analytics` | Manager dashboard statistics |
| `detectRoutes.js` | `/api/detect` | Forwards one image to the AI service |

After a successful create, update or delete of equipment, the route emits a Socket.io event so every connected screen updates.

The full endpoint and permission table is in the [backend README](../README.md#api-overview).
