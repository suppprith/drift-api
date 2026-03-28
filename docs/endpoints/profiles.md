# User Profiles Endpoints

> Phase 9: User Profiles

```
GET    /profiles/me                      → Current user profile + stats
PATCH  /profiles/me                      → Update profile
GET    /profiles/:username               → Public profile
GET    /profiles/me/trips                → My trips (paginated, filtered)
GET    /profiles/me/saved                → Saved trips
GET    /profiles/me/notifications        → Notifications (paginated)
PATCH  /profiles/me/notifications/read   → Mark notifications as read
GET    /profiles/me/stats                → Travel stats (countries, trips, XP, etc.)
PUT    /profiles/me/avatar               → Upload avatar to R2
```
