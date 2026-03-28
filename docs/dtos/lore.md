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

## Group Challenge DTOs

```typescript
const CreateGroupChallengeRequest = z.object({
  challenge_type: z.enum([
    "race",
    "collective",
    "versus",
    "scavenger_hunt",
    "streak",
  ]),
  title: z.string().min(1).max(200),
  description: z.string().max(1000).optional(),
  emoji: z.string().optional(),
  target_type: z.enum(["quest", "checkin", "photo", "activity", "custom"]),
  target_count: z.number().int().min(1).max(100).default(1),
  target_quest_ids: z.array(z.string().uuid()).optional(),
  deadline: z.string().datetime().optional(),
  xp_reward: z.number().int().min(0).max(1000).default(100),
});

const GroupChallengeResponse = z.object({
  id: z.string().uuid(),
  trip_id: z.string().uuid(),
  created_by: z.object({
    id: z.string().uuid(),
    username: z.string(),
    display_name: z.string(),
    avatar_url: z.string().nullable(),
  }),
  challenge_type: z.enum([
    "race",
    "collective",
    "versus",
    "scavenger_hunt",
    "streak",
  ]),
  title: z.string(),
  description: z.string().nullable(),
  emoji: z.string().nullable(),
  target_type: z.string(),
  target_count: z.number(),
  deadline: z.string().nullable(),
  xp_reward: z.number(),
  status: z.enum(["active", "completed", "expired", "cancelled"]),
  winner: z
    .object({
      id: z.string().uuid(),
      username: z.string(),
      display_name: z.string(),
    })
    .nullable(),
  progress: z.array(
    z.object({
      user: z.object({
        id: z.string().uuid(),
        username: z.string(),
        display_name: z.string(),
        avatar_url: z.string().nullable(),
      }),
      progress: z.number(),
      completed: z.boolean(),
      completed_at: z.string().nullable(),
    }),
  ),
  created_at: z.string(),
});
```
