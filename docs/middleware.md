# Middleware Stack

Order matters — applied top to bottom.

```typescript
// src/index.ts — middleware application order
app.use("*", requestId()); // 1. Inject X-Request-ID
app.use("*", timing()); // 2. Server-Timing header
app.use("*", logger()); // 3. Request logging
app.use("*", secureHeaders()); // 4. Security headers (CSP, HSTS, etc.)
app.use("*", corsMiddleware()); // 5. CORS (configured per environment)
app.use("*", errorHandler()); // 6. Global error handler + Sentry
app.use("/api/*", rateLimiter()); // 7. Rate limiting (Redis-backed)
app.use("/api/protected/*", auth()); // 8. JWT validation (Supabase JWKS)
app.use("*", bodyLimit({ maxSize: 10 * 1024 * 1024 })); // 9. 10MB body limit
```

---

## Rate Limiting Strategy (Redis Sliding Window)

```
Anonymous users:  60 requests/minute, 0 generations (no token)
Guest sessions:   60 requests/minute, 1 generation/day, 3 swaps/trip, 1 active trip
Free accounts:    120 requests/minute, 5 generations/day
Pro accounts:     300 requests/minute, unlimited generations
Admin:            No limits
```

### Implementation

Redis `INCR` + `EXPIRE` (sliding window counter).

```
Key: drift:rate:{userId|ip}:{endpoint_group}:{window}
```
