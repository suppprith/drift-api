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

## Trip Collaboration

```
POST   /trips/:id/invite/:userId        → Invite a friend to join the trip
POST   /trips/:id/request-join          → Request to join a friend's trip
PATCH  /trips/:id/members/:memberId     → Accept/decline invite or join request
DELETE /trips/:id/members/:memberId     → Remove member or leave trip
GET    /trips/:id/members               → List all trip members + roles
PATCH  /trips/:id/members/:memberId/role → Change member role (owner only)
```

## Trip History & Memories

```
GET    /trips/history                    → List completed trips with memory summaries (auth, paginated)
GET    /trips/:id/memories               → Full memory view of a completed trip (auth, owner or member)
POST   /trips/:id/complete               → Mark trip as completed, trigger memory generation (auth)
```

## Activity Feedback

```
POST   /trips/:id/activities/:activityId/feedback → Rate an activity (loved/liked/neutral/disliked) (auth)
GET    /trips/:id/feedback               → Get all feedback for a trip (auth, owner or member)
DELETE /trips/:id/activities/:activityId/feedback → Remove feedback (auth)
```

## Trip Photos

```
POST   /trips/:id/photos                 → Upload a trip photo (auth)
GET    /trips/:id/photos                 → Get all photos for a trip (auth, owner or member)
DELETE /trips/:id/photos/:photoId        → Delete a photo (auth, owner only)
```
