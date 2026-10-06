# SafeCross – Frontend

React single-page application with a dashboard for each user role. It talks to the backend over REST and receives live updates through Socket.io.

## Tech

React · Vite · Tailwind CSS · React Router · Socket.io client · Recharts · React-Leaflet

## Structure

| Path | Purpose |
|---|---|
| `src/main.jsx` | Application entry point |
| `src/App.jsx` | Route definitions |
| [`src/pages/`](src/pages) | One page per screen: login, four dashboards, junction details |
| `src/components/` | Shared components (loading spinner) |
| `src/data/` | Static sample data |
| `src/index.css` | Tailwind base styles |
| `vercel.json` | Rewrite rule so client-side routes work on Vercel |

## Setup

```bash
npm install
npm run dev
```

Create a `.env` file in this folder:

```
VITE_API_URL=http://localhost:3000/api
```

In production, set `VITE_API_URL` to the deployed backend address, ending with `/api`. Vite reads it at build time, so redeploy after changing it.

## Scripts

| Command | What it does |
|---|---|
| `npm run dev` | Start the development server (http://localhost:5173) |
| `npm run build` | Build for production into `dist/` |
| `npm run preview` | Preview the production build |
| `npm run lint` | Run ESLint |

## Routes

| Path | Screen | Role |
|---|---|---|
| `/` | Login | Everyone |
| `/admin` | Users and hardware infrastructure | Admin |
| `/manager` | Safety analytics | Manager |
| `/dispatcher` | Live map and alert table | Dispatcher |
| `/technician` | Faulty equipment | Technician |
| `/crosswalk/:id` | Junction details and alert history | Dispatcher |

The login response includes the user's role, and the app sends the user to the matching dashboard. Access control is enforced by the backend on every request.
