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

## Auth

```
POST   /auth/signup                      → Sign up with email/password (+ migrate guest if X-Guest-Token)
POST   /auth/login                       → Login, returns JWT + refresh token (+ migrate guest if X-Guest-Token)
POST   /auth/logout                      → Invalidate session
POST   /auth/refresh                     → Refresh JWT
POST   /auth/oauth/google               → Google OAuth callback
POST   /auth/oauth/apple                → Apple OAuth callback
POST   /auth/forgot-password            → Send password reset email
POST   /auth/reset-password             → Reset password with token
DELETE /auth/account                     → Delete account + all data (GDPR)
```
