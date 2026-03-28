# Drift Lore & Gamification Endpoints

> Phase 7: Drift Lore & Gamification

## Quests

```
GET    /lore/quests?destination=&type=  → Available quests for destination
GET    /lore/quests/:id                 → Quest details
GET    /lore/quests/nearby?lat=&lng=    → Quests within geofence radius
GET    /lore/quests/active/:tripId      → Quests available during current trip
```

## Photo Proof & Completion

```
POST   /lore/complete                    → Submit quest completion with photo
GET    /lore/completions/me             → My completed quests
GET    /lore/completions/:id            → Single completion details
POST   /lore/completions/:id/verify     → Admin: manually verify/reject
```

## Gamification

```
GET    /gamification/me                  → My XP, level, progress to next
GET    /gamification/leaderboard?scope= → Global or per-destination leaderboard
GET    /gamification/achievements/me    → My unlocked achievements
GET    /gamification/rewards/me         → My available discounts/rewards
POST   /gamification/rewards/:id/redeem → Redeem a reward
```
