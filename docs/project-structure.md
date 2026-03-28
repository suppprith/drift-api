# Project Structure

```
drift-api/
├── src/
│   ├── index.ts                          # Hono app entry, middleware chain, route mounting
│   │
│   ├── routes/                           # Feature-based route modules
│   │   ├── health.ts                     # GET /health, GET /health/detailed
│   │   ├── auth/
│   │   │   ├── index.ts                  # POST /auth/signup, /auth/login, /auth/logout, /auth/refresh
│   │   │   ├── schemas.ts               # Zod schemas for auth DTOs
│   │   │   └── handlers.ts              # Handler functions
│   │   ├── onboarding/
│   │   │   ├── index.ts                  # Onboarding flow endpoints
│   │   │   ├── schemas.ts               # Destination search, transport, profile schemas
│   │   │   └── handlers.ts
│   │   ├── destinations/
│   │   │   ├── index.ts                  # Destination data endpoints
│   │   │   ├── schemas.ts               # Destination, interest, must-do schemas
│   │   │   └── handlers.ts
│   │   ├── trips/
│   │   │   ├── index.ts                  # Trip CRUD + generation
│   │   │   ├── schemas.ts               # Trip, itinerary, activity schemas
│   │   │   └── handlers.ts
│   │   ├── itinerary/
│   │   │   ├── index.ts                  # Itinerary editing endpoints
│   │   │   ├── schemas.ts               # Edit, swap, regenerate schemas
│   │   │   └── handlers.ts
│   │   ├── booking/
│   │   │   ├── index.ts                  # Booking cart + checkout
│   │   │   ├── schemas.ts               # Booking item, cart schemas
│   │   │   └── handlers.ts
│   │   ├── lore/
│   │   │   ├── index.ts                  # Drift Lore challenges + quests
│   │   │   ├── schemas.ts               # Quest, challenge, photo proof schemas
│   │   │   └── handlers.ts
│   │   ├── social/
│   │   │   ├── index.ts                  # Feed, upvotes, saves, comments
│   │   │   ├── schemas.ts               # Social interaction schemas
│   │   │   └── handlers.ts
│   │   ├── profiles/
│   │   │   ├── index.ts                  # User profile, level, stats
│   │   │   ├── schemas.ts               # Profile, stats schemas
│   │   │   └── handlers.ts
│   │   ├── library/
│   │   │   ├── index.ts                  # Public itinerary library
│   │   │   ├── schemas.ts               # Search, filter, pagination schemas
│   │   │   └── handlers.ts
│   │   ├── live/
│   │   │   ├── index.ts                  # Live trip companion endpoints
│   │   │   ├── schemas.ts               # Check-in, notification schemas
│   │   │   └── handlers.ts
│   │   ├── export/
│   │   │   ├── index.ts                  # PDF export, share links
│   │   │   ├── schemas.ts
│   │   │   └── handlers.ts
│   │   └── admin/
│   │       ├── index.ts                  # Admin/moderation endpoints
│   │       ├── schemas.ts
│   │       └── handlers.ts
│   │
│   ├── middleware/
│   │   ├── auth.ts                       # Supabase JWT validation via JWKS
│   │   ├── rateLimit.ts                  # Redis sliding-window rate limiter
│   │   ├── cors.ts                       # CORS configuration
│   │   ├── errorHandler.ts              # Global error handler + Sentry
│   │   ├── requestId.ts                 # Unique request ID injection
│   │   ├── timing.ts                    # Server-Timing header
│   │   └── validation.ts               # Zod validation hook for OpenAPI
│   │
│   ├── services/
│   │   ├── ai/
│   │   │   ├── gemini.ts                # Gemini client wrapper (text + vision)
│   │   │   ├── prompts/
│   │   │   │   ├── system.ts            # Core system prompt (local-first rules)
│   │   │   │   ├── itinerary.ts         # Generation prompt template
│   │   │   │   ├── interests.ts         # Destination interest generation
│   │   │   │   ├── mustDos.ts           # Must-do experience generation
│   │   │   │   ├── alternatives.ts      # Activity swap alternatives
│   │   │   │   ├── loreQuests.ts        # Lore challenge generation
│   │   │   │   └── photoVerify.ts       # Photo proof verification prompt
│   │   │   ├── imageRecognition.ts      # Vision pipeline: landmark + scene + moderation
│   │   │   ├── embeddings.ts            # Text embeddings for semantic search
│   │   │   └── streaming.ts            # SSE streaming utilities
│   │   ├── cache/
│   │   │   ├── redis.ts                 # Upstash Redis client wrapper
│   │   │   ├── layers.ts               # Multi-layer cache: Redis → Supabase → API
│   │   │   └── keys.ts                 # Cache key naming conventions
│   │   ├── data/
│   │   │   ├── supabase.ts             # Supabase client factory
│   │   │   ├── trips.ts               # Trip data access
│   │   │   ├── itineraries.ts         # Itinerary data access
│   │   │   ├── profiles.ts           # User profile data access
│   │   │   ├── lore.ts               # Lore/quest data access
│   │   │   └── social.ts             # Social interactions data access
│   │   ├── external/
│   │   │   ├── mapbox.ts              # Geocoding + static map images
│   │   │   ├── weather.ts            # Open-Meteo client
│   │   │   ├── currency.ts           # ExchangeRate-API client
│   │   │   ├── reddit.ts             # Reddit OAuth + destination intel
│   │   │   ├── tripadvisor.ts        # TripAdvisor Content API enrichment
│   │   │   └── cloudVision.ts        # Google Cloud Vision SafeSearch
│   │   ├── queue/
│   │   │   ├── qstash.ts             # Upstash QStash client
│   │   │   └── jobs.ts               # Background job definitions
│   │   ├── gamification/
│   │   │   ├── xp.ts                 # XP calculation and level progression
│   │   │   ├── leaderboard.ts        # Redis sorted set leaderboards
│   │   │   ├── rewards.ts            # Reward/discount engine
│   │   │   └── achievements.ts       # Achievement badge system
│   │   └── notifications/
│   │       ├── push.ts               # Push notification service
│   │       └── inApp.ts              # In-app notification manager
│   │
│   ├── schemas/
│   │   ├── common.ts                  # Shared Zod schemas (pagination, errors, etc.)
│   │   ├── enums.ts                   # All enum definitions
│   │   └── database.ts               # Database row type schemas
│   │
│   ├── types/
│   │   ├── env.ts                     # Cloudflare Worker bindings type
│   │   ├── context.ts                # Hono context variables type
│   │   └── api.ts                    # Shared API types
│   │
│   └── utils/
│       ├── crypto.ts                  # Hashing, token generation
│       ├── slug.ts                    # URL slug generation
│       ├── geo.ts                    # Geofencing, distance calculations
│       ├── currency.ts              # Currency formatting/conversion
│       └── dates.ts                 # Date/time utilities
│
├── wrangler.toml                     # Cloudflare Workers configuration
├── package.json
├── tsconfig.json
├── .env.example                      # Environment variable template
└── vitest.config.ts                  # Test configuration
```
