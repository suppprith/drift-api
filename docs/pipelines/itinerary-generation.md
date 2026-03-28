# Itinerary Generation Pipeline

```
User presses "Generate"
         │
         ▼
┌─ ASSEMBLE CONTEXT (parallel, all from cache) ─────────┐
│  • Trip preferences (from DB)                          │
│  • Destination research (Redis → Supabase cache)       │
│  • Reddit intel (Redis → Supabase cache)               │
│  • Weather data (Redis → Open-Meteo)                   │
│  • Exchange rates (Redis → ExchangeRate-API)            │
│  • Must-dos for destination (Redis → Supabase cache)   │
│  • Selected interests                                   │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ BUILD GEMINI PROMPT ──────────────────────────────────┐
│  System prompt (local-first rules, audit checklist)    │
│  + Context payload (preferences + research + weather)  │
│  + Output schema (strict JSON, Zod-derived)            │
│  + Self-audit instructions (replaces 2nd pass)         │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ GEMINI 2.5 FLASH STREAMING ──────────────────────────┐
│  • responseModalities: ['TEXT']                         │
│  • responseMimeType: 'application/json'                │
│  • responseSchema: itineraryJsonSchema                 │
│  • Stream chunks → parse day-by-day → emit SSE events  │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ POST-PROCESSING (parallel per day) ──────────────────┐
│  • Geocode each activity → Mapbox → validate/fix latlng│
│  • TripAdvisor enrichment → rating + review count      │
│  • Must-do cross-reference → flag missing experiences  │
│  • Compute running cost totals                          │
│  • Generate transit polylines between activities        │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ SAVE + EMIT ──────────────────────────────────────────┐
│  • Batch insert days + activities + transit to Supabase │
│  • Cache trip in Redis (1hr TTL)                        │
│  • Final SSE event: { type: 'complete', slug, totals } │
│  • Async: queue TripAdvisor enrichment if cache miss    │
└────────────────────────────────────────────────────────┘
```
