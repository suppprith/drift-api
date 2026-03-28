# Library DTOs

> `routes/library/schemas.ts`

## LibrarySearchQuery

```typescript
const LibrarySearchQuery = z.object({
  q: z.string().max(200).optional(),
  destination: z.string().optional(),
  duration_min: z.coerce.number().int().optional(),
  duration_max: z.coerce.number().int().optional(),
  budget_tier: z.enum(["budget", "mid-range", "luxury"]).optional(),
  vibes: z.string().optional(), // comma-separated: "adventure,food"
  season: z.enum(["spring", "summer", "autumn", "winter"]).optional(),
  sort: z.enum(["popular", "recent", "trending"]).default("popular"),
  page: z.coerce.number().int().min(1).default(1),
  limit: z.coerce.number().int().min(1).max(50).default(20),
});
```

## LibraryTripCard

```typescript
const LibraryTripCard = z.object({
  id: z.string().uuid(),
  slug: z.string(),
  title: z.string(),
  destinations: z.array(z.object({ city: z.string(), country: z.string() })),
  duration_days: z.number(),
  budget_tier: z.string(),
  vibes: z.array(z.string()),
  estimated_total_cost_usd: z.number().nullable(),
  upvote_count: z.number(),
  save_count: z.number(),
  user: z
    .object({
      username: z.string(),
      display_name: z.string(),
      avatar_url: z.string().nullable(),
      level_name: z.string(),
    })
    .nullable(),
  cover_image_url: z.string().nullable(),
  created_at: z.string(),
});
```
