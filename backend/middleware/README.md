# middleware

Functions that run before a route handler and decide whether the request may continue.

| File | Purpose | On failure |
|---|---|---|
| `authenticationMiddleware.js` | Verifies the JWT from `Authorization: Bearer <token>` and attaches the user to `req.user` | 401 |
| `authorizationMiddleware.js` | Checks that `req.user.role` is one of the roles allowed for the route | 403 |
| `apiKeyMiddleware.js` | Verifies the `x-api-key` header on machine endpoints (the AI service has no user token) | 401 |

Typical use in a route:

```js
router.post('/', authenticate, authorize('Admin'), handler);   // user route
router.post('/', apiKey, handler);                             // sensor route
```
