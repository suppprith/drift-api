# Itinerary Generation Pipeline

## Design Philosophy: Dynamic-First, Never Generic

Every itinerary is generated fresh by AI with maximum user-specific context. The system avoids pre-stored templates or one-size-fits-all suggestions. Destination research and Reddit intel serve as raw material for the AI — but the actual recommendations are dynamically shaped by each user's travel history, preferences, past feedback, and behavioral signals. Two users going to the same city on the same dates will get meaningfully different itineraries.

## Pipeline

```
User presses "Generate"
         │
         ▼
┌─ ASSEMBLE CONTEXT (parallel) ─────────────────────────┐
│                                                        │
│  ── Destination Context (shared, from cache) ──        │
│  • Destination research (Redis → PostgreSQL cache)       │
│  • Reddit intel (Redis → PostgreSQL cache)               │
│  • TripAdvisor intel (Redis → PostgreSQL cache)         │
│    → Top-rated attractions, restaurants, experiences    │
│    → Ratings, review counts, traveler sentiment        │
│    → Category breakdown (things to do, dining, etc.)   │
│  • Weather data (Redis → Open-Meteo)                   │
│  • Exchange rates (Redis → ExchangeRate-API)            │
│                                                        │
│  ── User-Specific Context (per-user, from DB) ──       │
│  • Trip preferences (current trip)                     │
│  • Selected interests + must-dos                       │
│  • Travel DNA (profiles.travel_dna — evolved prefs)    │
│  • Past trip summaries (last 10 completed trips)       │
│  • Activity feedback history (liked/disliked patterns) │
│  • Previously visited places (avoid repeats)           │
│  • Quest completion patterns (adventure level)         │
│  • Skipped activities from past live trips             │
│  • Friend travel overlap (if traveling with friends)   │
│                                                        │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ BUILD PERSONALIZED PROMPT ────────────────────────────┐
│  System prompt:                                        │
│    • Local-first rules, audit checklist                │
│    • "Never suggest places this user already visited"  │
│    • "Weight toward categories they consistently like" │
│    • "Avoid categories they've skipped or disliked"    │
│    • "Match their actual pace from past trips"         │
│                                                        │
│  Context payload:                                      │
│    • Destination research + Reddit (raw material)      │
│    • TripAdvisor intel (ratings, reviews, top spots)   │
│    • Weather + exchange rates (hard facts)             │
│    • User travel DNA (personality profile)             │
│    • Past feedback signals (likes, skips, ratings)     │
│    • Trip-specific preferences (current trip)          │
│                                                        │
│  Output schema: strict JSON (Zod-derived)              │
│  Self-audit: replaces 2nd LLM pass                     │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ GEMINI 2.5 FLASH STREAMING ──────────────────────────┐
│  • responseModalities: ['TEXT']                         │
│  • responseMimeType: 'application/json'                │
│  • responseSchema: itineraryJsonSchema                 │
│  • Stream chunks → parse day-by-day → emit SSE events  │
│  • Each activity includes a personalization_reason      │
│    explaining why this was picked for this user         │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ POST-PROCESSING (parallel per day) ──────────────────┐
│  • Geocode each activity → Mapbox → validate/fix latlng│
│  • TripAdvisor deep enrichment → per-activity URL,     │
│    exact review count, photos (supplements pre-gen     │
│    context with activity-specific detail)               │
│  • Must-do cross-reference → flag missing experiences  │
│  • Repeat filter → reject any activity at a place the  │
│    user has already checked in or visited               │
│  • Compute running cost totals                          │
│  • Generate transit polylines between activities        │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ SAVE + EMIT ──────────────────────────────────────────┐
│  • Batch insert days + activities + transit to PostgreSQL │
│  • Cache trip in Redis (1hr TTL, keyed per user+trip)   │
│  • Final SSE event: { type: 'complete', slug, totals } │
│  • Async: queue TripAdvisor enrichment if cache miss    │
└────────────────────────────────────────────────────────┘
```

## What Makes Each Trip Unique

| Signal                  | How It's Used                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Past trips**          | AI avoids suggesting the same restaurants, landmarks, or neighborhoods the user has already visited           |
| **Activity feedback**   | "Loved" activities bias the AI toward that category; "disliked" activities steer it away                      |
| **Skipped activities**  | Activities skipped during live trips are treated as soft negative signals                                     |
| **Check-in duration**   | Long check-ins at a type of place (e.g., art galleries) increase that category's weight                       |
| **Quest patterns**      | Users who complete adventure quests get more adventurous suggestions; food quest lovers get more food spots   |
| **Travel pace history** | If users consistently skip late-night activities, the AI learns to end days earlier                           |
| **Group composition**   | Couples get different vibes than friend groups, even at the same destination                                  |
| **Travel DNA**          | A JSONB profile that evolves after each trip — preferred categories, price sensitivity, pace, adventure level |

## Travel DNA Evolution

After each completed trip, the system updates the user's `travel_dna` in their profile:

```
Trip completed
      │
      ▼
┌─ ANALYZE TRIP SIGNALS ────────────────────────┐
│  • Which activities were checked into?         │
│  • Which were skipped?                         │
│  • Activity feedback ratings                   │
│  • Time spent at each check-in                 │
│  • Category distribution of liked activities   │
│  • Budget actuals vs estimates                 │
│  • Pace (activities per day actually done)     │
└────────────────────────────────────────────────┘
      │
      ▼
┌─ UPDATE TRAVEL DNA (Gemini) ──────────────────┐
│  Feed old travel_dna + new trip signals        │
│  → Gemini outputs updated travel_dna JSON      │
│  → Merge into profiles.travel_dna              │
└────────────────────────────────────────────────┘
```

### Travel DNA Schema (JSONB)

```json
{
  "top_categories": ["food", "culture", "nature"],
  "avoided_categories": ["shopping", "nightlife"],
  "pace_preference": "balanced",
  "avg_activities_per_day": 5.2,
  "budget_tendency": "mid-range",
  "adventure_level": 0.7,
  "food_adventurousness": 0.9,
  "early_bird_vs_night_owl": "early_bird",
  "walking_tolerance_km": 8.5,
  "tourist_trap_tolerance": "low",
  "hidden_gem_affinity": "high",
  "repeat_visit_tendency": "low",
  "trips_analyzed": 6,
  "last_updated": "2026-03-15T00:00:00Z"
}
```
