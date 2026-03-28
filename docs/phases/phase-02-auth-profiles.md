# Phase 2: Auth + Profiles (Week 1-2)

_Parallel with Phase 1_

## Tasks

1. Configure Better Auth with email/password + Google OAuth
2. Auth routes (signup, login, OAuth, refresh, logout) via Better Auth handlers
3. Profile CRUD routes
4. Avatar upload to S3 MinIO
5. Session validation middleware (Better Auth)

## Relevant Files

- `src/routes/auth/*` — auth routes + schemas
- `src/routes/profiles/*` — profile routes
- `src/middleware/auth.ts` — Better Auth session validation

## Related Docs

- [Auth Endpoints](../endpoints/auth.md)
- [Auth DTOs](../dtos/auth.md)
- [Profiles Endpoints](../endpoints/profiles.md)
- [Profiles DTOs](../dtos/profiles.md)
