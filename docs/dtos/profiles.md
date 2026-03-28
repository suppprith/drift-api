# Profiles DTOs

> `routes/profiles/schemas.ts`

## UpdateProfileRequest

```typescript
const UpdateProfileRequest = z.object({
  display_name: z.string().min(1).max(100).optional(),
  username: z
    .string()
    .min(3)
    .max(30)
    .regex(/^[a-z0-9_]+$/)
    .optional(),
  home_city: z.string().max(100).optional(),
  home_country: z.string().max(100).optional(),
  travel_style_summary: z.string().max(500).optional(),
});
```

## ProfileResponse

```typescript
const ProfileResponse = z.object({
  id: z.string().uuid(),
  username: z.string(),
  display_name: z.string(),
  avatar_url: z.string().nullable(),
  home_city: z.string().nullable(),
  home_country: z.string().nullable(),
  travel_style_summary: z.string().nullable(),
  xp: z.number(),
  level: z.number(),
  level_name: z.string(),
  total_trips: z.number(),
  total_countries: z.number(),
  total_lore_completed: z.number(),
  is_pro: z.boolean(),
  created_at: z.string(),
});
```

## TravelStatsResponse

```typescript
const TravelStatsResponse = z.object({
  total_trips: z.number(),
  total_countries: z.number(),
  total_cities: z.number(),
  total_days_traveled: z.number(),
  total_activities: z.number(),
  total_lore_completed: z.number(),
  favorite_vibes: z.array(z.string()),
  countries_visited: z.array(z.string()),
  most_visited_destination: z.string().nullable(),
  member_since: z.string(),
});
```

## NotificationResponse

```typescript
const NotificationResponse = z.object({
  id: z.string().uuid(),
  type: z.string(),
  title: z.string(),
  body: z.string().nullable(),
  data: z.record(z.any()),
  is_read: z.boolean(),
  created_at: z.string(),
});
```
