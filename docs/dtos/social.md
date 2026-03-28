# Social & Feed DTOs

> `routes/social/schemas.ts`

## FeedItemResponse

```typescript
const FeedItemResponse = z.object({
  id: z.string().uuid(),
  type: z.enum(["quest_completion", "trip_shared", "level_up", "achievement"]),
  user: z.object({
    username: z.string(),
    display_name: z.string(),
    avatar_url: z.string().nullable(),
    level_name: z.string(),
  }),
  content: z.union([
    QuestCompletionResponse,
    TripDetailResponse.pick({
      id: true,
      slug: true,
      title: true,
      destinations: true,
      duration_days: true,
      budget_tier: true,
      vibes: true,
    }),
  ]),
  created_at: z.string(),
  like_count: z.number(),
  comment_count: z.number(),
  is_liked_by_me: z.boolean(),
});
```

## CommentRequest

```typescript
const CommentRequest = z.object({
  target_type: z.enum(["trip", "quest_completion", "comment"]),
  target_id: z.string().uuid(),
  body: z.string().min(1).max(2000),
  parent_id: z.string().uuid().optional(), // for replies
});
```

## ReportRequest

```typescript
const ReportRequest = z.object({
  target_type: z.enum(["trip", "quest_completion", "comment", "profile"]),
  target_id: z.string().uuid(),
  reason: z.enum([
    "spam",
    "inappropriate",
    "inaccurate",
    "harassment",
    "other",
  ]),
  description: z.string().max(1000).optional(),
});
```
