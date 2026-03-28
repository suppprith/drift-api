# Environment Variables

```env
# PostgreSQL
DATABASE_URL=postgresql://user:password@host:5432/drift

# MinIO (S3-compatible)
MINIO_ENDPOINT=
MINIO_ACCESS_KEY=
MINIO_SECRET_KEY=
MINIO_BUCKET_NAME=drift-files
MINIO_PUBLIC_URL=

# Auth (Better Auth)
BETTER_AUTH_SECRET=
BETTER_AUTH_URL=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=

# Upstash Redis
UPSTASH_REDIS_REST_URL=
UPSTASH_REDIS_REST_TOKEN=

# Upstash QStash
QSTASH_URL=
QSTASH_TOKEN=

# AI
GEMINI_API_KEY=
GOOGLE_CLOUD_VISION_API_KEY=

# Maps
MAPBOX_ACCESS_TOKEN=        # public (scoped to domains)
MAPBOX_SECRET_TOKEN=        # server-side geocoding

# External APIs
REDDIT_CLIENT_ID=
REDDIT_CLIENT_SECRET=
REDDIT_USER_AGENT=drift-travel-app/1.0
TRIPADVISOR_API_KEY=
EXCHANGERATE_API_KEY=

# Cloudflare

# Sentry
SENTRY_DSN=

# App
APP_ENV=development|staging|production
APP_URL=https://drift.travel
CORS_ORIGINS=https://drift.travel,http://localhost:3000
```
