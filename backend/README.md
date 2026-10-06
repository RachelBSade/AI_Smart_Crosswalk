# SafeCross – Backend

Node.js and Express server that exposes the REST API and the Socket.io real-time channel, stores data in MongoDB Atlas and uploads incident snapshots to Cloudinary.

## Structure

| Path | Purpose |
|---|---|
| `index.js` | Entry point: HTTP server, Socket.io, database connection, change streams |
| `app.js` | Express app: CORS, JSON parsing and route mounting |
| [`config/`](config) | Database, Socket.io and Cloudinary setup |
| [`middleware/`](middleware) | JWT authentication, role authorization, sensor API key |
| [`models/`](models) | Mongoose schemas |
| [`routes/`](routes) | REST endpoints |
| [`services/`](services) | Business logic |
| [`tests/`](tests) | Seed script and end-to-end tests |
| [`ai-service/`](ai-service) | Python video analysis service |

A request flows through the layers in this order: **route → middleware → service → model**.

## Setup

```bash
npm install
cp .env.example .env
npm run seed
npm start
```

## Environment variables

| Variable | Description |
|---|---|
| `PORT` | Server port (default 3000) |
| `MONGO_URI` | MongoDB Atlas connection string |
| `JWT_SECRET` | Secret used to sign login tokens |
| `SENSOR_API_KEY` | Shared key the AI service sends in the `x-api-key` header |
| `FRONTEND_URL` | Allowed frontend origin for CORS (no trailing slash) |
| `AI_SERVICE_URL` | Address of the Python FastAPI service |
| `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET` | Cloudinary credentials |

## Scripts

| Command | What it does |
|---|---|
| `npm start` | Start the server |
| `npm run seed` | Insert demo users and demo data (safe to run more than once) |
| `npm run test:e2e` | Run the end-to-end test suite on an in-memory MongoDB |

## API overview

All routes are under `/api`. User routes need `Authorization: Bearer <token>`.

| Resource | Endpoints | Access |
|---|---|---|
| Users | `POST /users/login` | Public |
| | `POST /users/register`, `GET /users`, `PUT /users/:id`, `PATCH /users/:id/status`, `DELETE /users/:id` | Admin |
| Crosswalks, cameras, LEDs | `GET` | All roles |
| | `POST`, `DELETE /:id` | Admin |
| | `PUT /:id` | Admin, Technician |
| Alerts | `POST /alerts` | AI service (`x-api-key`) |
| | `GET /alerts` | All roles |
| | `PUT /alerts/:id` | Admin, Dispatcher, Technician |
| Analytics | `GET /analytics/dashboard?filter=top5\|school\|all` | Manager, Admin |
| Detect | `POST /detect` | AI service (`x-api-key`) |

## Real-time events (Socket.io)

| Event | Sent when |
|---|---|
| `newAlert`, `alertUpdated` | An alert is created or changed |
| `alert_resolved`, `alert_reopened` | A dispatcher handles or re-opens an alert |
| `infra_added`, `infra_updated`, `infra_deleted` | A junction, camera or LED changes (payload `{ type, payload }`) |
| `user_added`, `user_updated`, `user_deleted` | A user changes |
