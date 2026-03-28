# Admin & Moderation Endpoints

> Phase 12: Admin & Moderation

```
GET    /admin/reports                    → Pending reports queue
PATCH  /admin/reports/:id               → Resolve report
GET    /admin/stats                      → Platform stats (users, trips, revenue)
POST   /admin/quests                     → Create new lore quest
PATCH  /admin/quests/:id                → Update quest
DELETE /admin/quests/:id                → Remove quest
GET    /admin/moderation/photos         → Photos pending AI review
PATCH  /admin/moderation/photos/:id    → Approve/reject photo
POST   /admin/cache/invalidate         → Invalidate cache keys
GET    /admin/users                      → User management
PATCH  /admin/users/:id/ban            → Ban user
```
