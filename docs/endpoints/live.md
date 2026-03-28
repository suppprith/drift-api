# Live Trip Companion Endpoints

> Phase 6: Live Trip Companion

```
POST   /live/start/:tripId              → Activate live mode for trip
POST   /live/checkin                     → Location check-in (lat/lng)
GET    /live/:tripId/status             → Current activity, next up, ETAs
POST   /live/:tripId/skip-activity      → Skip current, recalculate day
POST   /live/:tripId/running-late       → AI recalculates remaining day
GET    /live/:tripId/nearby             → Nearby POIs not in itinerary
WS     /live/:tripId/ws                 → WebSocket for real-time updates (group sync)
```
