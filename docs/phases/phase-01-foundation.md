# Phase 1: Foundation (Week 1)

## Tasks

1. Initialize Hono project with TypeScript + wrangler.toml
2. Set up folder structure as defined in [project-structure.md](../project-structure.md)
3. Configure Upstash Redis client wrapper
4. Configure Supabase client factory
5. Implement middleware stack (auth, CORS, rate limiting, error handler)
6. Implement health check endpoints
7. Set up Zod OpenAPI route pattern
8. Add all database migrations (see [migrations.md](../database/migrations.md))
9. Configure R2 bucket binding

## Relevant Files

- `src/index.ts` — app entry
- `src/middleware/*.ts` — all middleware
- `src/services/cache/redis.ts` — Upstash client
- `src/services/data/supabase.ts` — Supabase client
- `src/schemas/common.ts` — shared schemas
- `src/types/env.ts` — CF Worker bindings
- `wrangler.toml` — CF config
