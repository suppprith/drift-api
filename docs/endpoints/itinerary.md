# Itinerary Editing Endpoints

> Phase 4: Itinerary Editing

```
PATCH  /trips/:id/days/:dayId                    → Update day metadata
PATCH  /trips/:id/activities/:activityId         → Update activity details
POST   /trips/:id/days/:dayId/activities         → Add custom activity
DELETE /trips/:id/activities/:activityId         → Remove activity
PATCH  /trips/:id/days/:dayId/reorder            → Reorder activities within day
POST   /trips/:id/days/:dayId/move-activity      → Move activity to different day
GET    /trips/:id/activities/:activityId/alternatives → Get swap alternatives
```
