# Verification Checklist

1. `POST /trips` with full onboarding payload → verify trip created with all preferences stored
2. `POST /trips/:id/generate` → connect SSE, verify status messages stream, then day objects, then complete event
3. Verify generated itinerary has: local food per day, rain backups, realistic transit times, must-dos included, no tourist trap defaults
4. `GET /destinations/search?q=Pokhara` → verify Mapbox autocomplete returns results
5. `POST /destinations/pokhara-nepal/prefetch` → verify Reddit intel + destination research cached in Redis within 10s
6. `POST /lore/complete` with photo → verify image uploaded to R2, Gemini Vision verifies, XP granted, appears in feed
7. `GET /gamification/leaderboard?scope=global` → verify Redis sorted set returns ranked users
8. Rate limit: hit `/trips/:id/generate` 6x as free user → verify 429 on 6th request
9. `POST /library/clone/:tripId` → verify new trip created as draft with cloned itinerary
10. Test multi-layer cache: cold start (no cache) → first request hits Gemini + writes to Redis + Supabase → second request returns from Redis in <5ms
11. `GET /export/:tripId/pdf` → verify PDF URL returned from R2
12. WebSocket `/live/:tripId/ws` → verify real-time check-in events broadcast to connected group members
