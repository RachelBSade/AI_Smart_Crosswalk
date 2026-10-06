# tests

Seed data and automated tests for the backend.

| File | Purpose | How to run |
|---|---|---|
| `seed.js` | Inserts the demo users and demo data. Safe to run more than once | `npm run seed` |
| `dummy-data.json` | The demo junctions, cameras, LEDs and alerts used by the seed |  |
| `e2e.test.js` | End-to-end suite. Starts an in-memory MongoDB and the real server, logs in with real users and checks the API, permissions, socket events and analytics | `npm run test:e2e` |
| `setupTestData.js` | Creates the demo data needed by the extended runner | `node tests/setupTestData.js` |
| `smartwalk.e2e.js` | Extended black-box runner against a running backend and AI service; prints a PASS / FAIL / SKIP report | `node tests/smartwalk.e2e.js` |

Notes:

- `npm run test:e2e` needs no database or `.env`. The first run downloads a MongoDB binary.
- `smartwalk.e2e.js` needs the backend, MongoDB and the FastAPI service to be running, and the demo users to exist.
- The demo users are for development only. Change their passwords before any real deployment.
