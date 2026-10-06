# pages

One file per screen. Each dashboard loads its data from the backend with the user's token and listens to the Socket.io events it needs.

| File | Screen | Live events |
|---|---|---|
| `Login.jsx` | Login form; stores the token and routes by role |  |
| `AdminDashboard.jsx` | User management and hardware infrastructure (junctions, cameras, LEDs), with automatic address-to-coordinates lookup | `infra_*`, `user_*` |
| `ManagerDashboard.jsx` | KPI cards and charts with a Top 5 / School zones / All filter, and PDF export | `alert_resolved`, `alert_reopened`, `infra_*` |
| `DispatcherDashboard.jsx` | Interactive map and live alert table; mark alerts as handled | `newAlert`, `alertUpdated` |
| `TechnicianDashboard.jsx` | List of faulty equipment; mark items as fixed | `infra_added`, `infra_updated` |
| `CrosswalkDetails.jsx` | One junction: equipment status and alert history | `newAlert`, `alertUpdated`, `infra_updated` |
