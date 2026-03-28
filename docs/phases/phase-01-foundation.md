# Phase 1: Foundation (Week 1)

## Tasks

1. Initialize Hono project with TypeScript + wrangler.toml
2. Set up folder structure as defined in [project-structure.md](../project-structure.md)
3. Configure Upstash Redis client wrapper
4. Configure PostgreSQL client (pg / postgres.js)
5. Configure S3 MinIO client
6. Implement middleware stack (auth, CORS, rate limiting, error handler)
7. Implement health check endpoints
8. Set up Zod OpenAPI route pattern
9. Add all database migrations (see [migrations.md](../database/migrations.md))

## Relevant Files

- `src/index.ts` — app entry
- `src/middleware/*.ts` — all middleware
- `src/services/cache/redis.ts` — Upstash client
- `src/services/data/db.ts` — PostgreSQL client
- `src/services/data/s3.ts` — S3 MinIO client
- `src/schemas/common.ts` — shared schemas
- `src/types/env.ts` — CF Worker bindings
- `wrangler.toml` — CF config
