# Trips Endpoints

> Phase 3: Trip Generation

## Trips CRUD

```
POST   /trips                            → Create trip draft from onboarding data
GET    /trips/:id                        → Get trip with full itinerary
GET    /trips/by-slug/:slug              → Get trip by URL slug (public)
PATCH  /trips/:id                        → Update trip metadata
DELETE /trips/:id                        → Delete trip
GET    /trips/:id/summary               → Lightweight trip summary (for cards)
```

## AI Generation

```
POST   /trips/:id/generate              → Generate itinerary (SSE streaming)
POST   /trips/:id/regenerate-day/:day   → Regenerate single day (SSE)
POST   /trips/:id/swap-activity/:activityId → Get 3-4 alternatives
POST   /trips/:id/apply-swap            → Apply selected alternative
POST   /trips/:id/surprise-me           → Add hidden gems to each day
POST   /trips/:id/adjust-pace/:day      → AI redistributes activities
POST   /trips/:id/add-day               → Extend trip by one day
DELETE /trips/:id/remove-day/:day       → Remove a day, redistribute
```
