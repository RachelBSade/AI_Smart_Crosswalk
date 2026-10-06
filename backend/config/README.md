# config

Setup code for the external services the backend depends on.

| File | Purpose |
|---|---|
| `db.js` | Connects to MongoDB Atlas using `MONGO_URI` |
| `socket.js` | Creates the Socket.io server, provides the emit helpers, and watches MongoDB change streams to broadcast live events |
| `cloudinary.js` | Configures the Cloudinary SDK used to store incident snapshots |

All values come from environment variables. Nothing secret is written in these files.
