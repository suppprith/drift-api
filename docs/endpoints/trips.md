# Trips Endpoints

> Phase 3: Trip Generation

## Trips CRUD

```
POST   /trips                            → Create trip draft from onboarding data (auth OR guest)
GET    /trips/:id                        → Get trip with full itinerary (auth OR guest owns trip)
GET    /trips/by-slug/:slug              → Get trip by URL slug (public)
PATCH  /trips/:id                        → Update trip metadata
DELETE /trips/:id                        → Delete trip
GET    /trips/:id/summary               → Lightweight trip summary (for cards)
```

## AI Generation

```
POST   /trips/:id/generate              → Generate itinerary (SSE streaming) (auth OR guest, 1/day limit)
POST   /trips/:id/regenerate-day/:day   → Regenerate single day (SSE) (auth only)
POST   /trips/:id/swap-activity/:activityId → Get 3-4 alternatives (auth OR guest, 3 swaps max for guests)
POST   /trips/:id/apply-swap            → Apply selected alternative (auth OR guest)
POST   /trips/:id/surprise-me           → Add hidden gems to each day (auth only)
POST   /trips/:id/adjust-pace/:day      → AI redistributes activities (auth only)
POST   /trips/:id/add-day               → Extend trip by one day (auth only)
DELETE /trips/:id/remove-day/:day       → Remove a day, redistribute (auth only)
```
