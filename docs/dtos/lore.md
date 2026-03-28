# Lore & Gamification DTOs

> `routes/lore/schemas.ts`

## QuestResponse

```typescript
const QuestResponse = z.object({
  id: z.string().uuid(),
  quest_type: z.enum(["destination_lore", "touch_grass", "dad_lore", "mythic"]),
  title: z.string(),
  description: z.string(),
  emoji: z.string(),
  difficulty: z.enum(["easy", "medium", "hard", "mythic"]),
  xp_reward: z.number(),
  bonus_reward: z
    .object({
      type: z.string(),
      value: z.number(),
      description: z.string(),
    })
    .nullable(),
  estimated_time_minutes: z.number().nullable(),
  min_level: z.number(),
  completion_count: z.number(),
  geofence: CoordinatesSchema.extend({ radius_m: z.number() }).nullable(),
  is_completed_by_me: z.boolean().optional(),
});
```

## SubmitQuestCompletionRequest

```typescript
const SubmitQuestCompletionRequest = z.object({
  quest_id: z.string().uuid(),
  trip_id: z.string().uuid().optional(),
  photo: z.string(), // base64 or R2 presigned upload key
  caption: z.string().max(500).optional(),
  lat: z.number(),
  lng: z.number(),
});
```

## QuestCompletionResponse

```typescript
const QuestCompletionResponse = z.object({
  id: z.string().uuid(),
  quest: QuestResponse,
  user: z.object({
    username: z.string(),
    display_name: z.string(),
    avatar_url: z.string().nullable(),
    level_name: z.string(),
  }),
  photo_url: z.string(),
  caption: z.string().nullable(),
  ai_verification_status: z.string(),
  xp_granted: z.number(),
  like_count: z.number(),
  comment_count: z.number(),
  created_at: z.string(),
});
```

## GamificationProfileResponse

```typescript
const GamificationProfileResponse = z.object({
  xp: z.number(),
  level: z.number(),
  level_name: z.string(), // Wanderer, Pathfinder, Drifter, Legend
  xp_to_next_level: z.number(),
  xp_progress_percent: z.number(),
  total_quests_completed: z.number(),
  total_countries_visited: z.number(),
  total_trips: z.number(),
  achievements: z.array(
    z.object({
      key: z.string(),
      name: z.string(),
      icon: z.string(),
      unlocked_at: z.string(),
    }),
  ),
  available_rewards: z.array(
    z.object({
      id: z.string(),
      type: z.string(),
      description: z.string(),
      value: z.number(),
      expires_at: z.string().nullable(),
    }),
  ),
});
```

## LeaderboardResponse

```typescript
const LeaderboardResponse = z.object({
  scope: z.enum(["global", "destination", "friends"]),
  destination: z.string().nullable(),
  entries: z.array(
    z.object({
      rank: z.number(),
      user: z.object({
        username: z.string(),
        display_name: z.string(),
        avatar_url: z.string().nullable(),
        level_name: z.string(),
      }),
      xp: z.number(),
      quests_completed: z.number(),
    }),
  ),
  my_rank: z.number().nullable(),
});
```
