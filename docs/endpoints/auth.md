# Auth Endpoints

> Phase 1: Foundation

## Health & System

```
GET    /health                           → { status, version, uptime }
GET    /health/detailed                  → { status, db, redis, ai, weather } (admin only)
```

## Guest Access

```
POST   /auth/guest                       → Create guest session, returns guest_token (72h TTL)
```

See [Guest System](../guest-system.md) for full details.

## Auth (Better Auth)

Better Auth handles these routes automatically via its Hono integration (`app.all("/api/auth/*", betterAuth.handler)`):

```
POST   /api/auth/sign-up/email          → Sign up with email/password
POST   /api/auth/sign-in/email          → Login with email/password
POST   /api/auth/sign-in/social         → Google OAuth (provider: "google")
POST   /api/auth/sign-out               → End session
GET    /api/auth/get-session            → Get current session + user
POST   /api/auth/forget-password        → Send password reset email
POST   /api/auth/reset-password         → Reset password with token
POST   /api/auth/delete-user            → Delete account + all data (GDPR)
```

### Custom Auth Extensions

```
POST   /auth/guest                       → Create guest session (custom, not Better Auth)
POST   /auth/migrate-guest              → Migrate guest trip to authenticated user (+ X-Guest-Token)
```
