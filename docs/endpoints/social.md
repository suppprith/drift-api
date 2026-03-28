# Social & Community Endpoints

> Phase 8: Social & Community

## Lore Feed

```
GET    /feed                             → Paginated feed of quest completions + trip posts
GET    /feed/destination/:key           → Feed filtered by destination
GET    /feed/following                  → Feed from followed users
GET    /feed/trending                   → Trending completions
```

## Interactions

```
POST   /social/upvote/:targetType/:id   → Upvote trip or completion
DELETE /social/upvote/:targetType/:id   → Remove upvote
POST   /social/save/:tripId             → Save trip to collection
DELETE /social/save/:tripId             → Unsave
POST   /social/comment                   → Add comment
PATCH  /social/comment/:id              → Edit comment
DELETE /social/comment/:id              → Delete comment
POST   /social/report                    → Report content
POST   /social/follow/:userId           → Follow user
DELETE /social/follow/:userId           → Unfollow
GET    /social/followers/:userId        → User's followers
GET    /social/following/:userId        → Users they follow
```
