# Technology Stack

| Layer           | Technology                     | Why                                           |
| --------------- | ------------------------------ | --------------------------------------------- |
| Runtime         | Cloudflare Workers             | Edge-first, $0 free tier, global              |
| Framework       | Hono v4 + @hono/zod-openapi    | Fast, typed, auto OpenAPI docs                |
| Auth            | Better Auth                    | TypeScript auth lib, email + OAuth + sessions |
| Database        | PostgreSQL                     | Self-hosted, pgvector, full control           |
| Cache           | Upstash Redis (HTTP)           | Works in Workers, 500K cmds/mo free           |
| Queue           | Upstash QStash                 | HTTP-based job queue for async tasks          |
| AI - Text       | Gemini 2.5 Flash               | Free tier, structured JSON, streaming         |
| AI - Vision     | Gemini 2.5 Flash Vision        | Landmark detection, photo verification        |
| AI - Moderation | Google Cloud Vision SafeSearch | NSFW detection, 1K/mo free                    |
| AI - Embeddings | Gemini text-embedding          | Semantic search for itinerary library         |
| Maps            | Mapbox GL JS + Geocoding API   | 50K loads + 100K geocode free                 |
| Weather         | Open-Meteo                     | Free, no API key needed                       |
| Currency        | ExchangeRate-API               | 1500 req/mo free                              |
| File Storage    | S3 MinIO                       | Self-hosted, S3-compatible                    |
| Error Tracking  | Sentry                         | 5K events/mo free                             |
| Validation      | Zod                            | Runtime + compile-time type safety            |

---

## Why Not Custom ML Models?

No need for self-trained ML models in V1. Gemini 2.5 Flash Vision handles:

- Landmark identification (is this the Eiffel Tower?)
- Scene classification (is this outdoors? a temple? a market?)
- Photo-location verification (does this photo match the claimed location?)
- Content moderation (safe for community feed?)
- OCR for receipts/tickets

Google Cloud Vision supplements with SafeSearch (NSFW) and structured landmark detection with confidence scores. Custom ML would only be needed at massive scale (V3+) for niche tasks like "identify specific street food dishes."

---

## Why Redis (Upstash)?

1. **Session cache** — Store onboarding wizard state server-side (survives device switches)
2. **Rate limiting** — Sliding window counters per user/IP
3. **Generation queue** — FIFO queue for AI generation requests during peak load
4. **Real-time leaderboards** — Sorted sets for XP rankings (per destination, global)
5. **Pub/Sub** — Live trip companion notifications between group members
6. **Destination cache** — Hot cache layer above PostgreSQL for frequently accessed destinations
7. **API response cache** — Cache expensive computed responses (interest tiles, must-dos)
8. **Distributed locks** — Prevent duplicate generation requests for same trip
