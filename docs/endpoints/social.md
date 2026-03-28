# Social & Community Endpoints

> Phase 8: Social & Community

## Lore Feed

```
GET    /feed                             → Paginated feed of quest completions + trip posts
GET    /feed/destination/:key           → Feed filtered by destination
GET    /feed/following                  → Feed from followed users
GET    /feed/friends                    → Feed from friends only
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

## Friends

```
POST   /social/friends/request/:userId  → Send friend request
PATCH  /social/friends/respond/:friendshipId → Accept or decline request
DELETE /social/friends/:userId          → Remove friend / cancel request
GET    /social/friends                   → My friends list (accepted)
GET    /social/friends/requests          → Pending incoming requests
GET    /social/friends/sent              → Pending outgoing requests
GET    /social/friends/mutual/:userId    → Mutual friends with another user
POST   /social/friends/block/:userId     → Block user
DELETE /social/friends/block/:userId     → Unblock user
```
