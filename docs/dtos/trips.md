# Trips & Itinerary Response DTOs

> `routes/trips/schemas.ts`

## ActivityResponse

```typescript
const ActivityResponse = z.object({
  id: z.string().uuid(),
  start_time: z.string(), // "07:00"
  end_time: z.string().nullable(),
  duration_minutes: z.number().nullable(),
  category: z.enum([
    "food",
    "attraction",
    "experience",
    "nature",
    "transport",
    "accommodation",
    "shopping",
    "nightlife",
    "wellness",
    "culture",
  ]),
  name: z.string(),
  description: z.string(),
  address: z.string().nullable(),
  neighborhood: z.string().nullable(),
  lat: z.number(),
  lng: z.number(),
  cost_local: z.number().nullable(),
  cost_usd: z.number().nullable(),
  cost_currency: z.string().nullable(),
  payment_methods: z.array(z.string()),
  local_tip: z.string().nullable(),
  crowd_timing: z.string().nullable(),
  booking_urgency: z.string().nullable(),
  order_recommendation: z.string().nullable(),
  is_tourist_trap: z.boolean(),
  local_alternative: z.string().nullable(),
  is_local_pick: z.boolean(),
  rain_backup: z
    .object({
      name: z.string(),
      description: z.string(),
      lat: z.number(),
      lng: z.number(),
    })
    .nullable(),
  tripadvisor: z
    .object({
      rating: z.number(),
      review_count: z.number(),
      url: z.string(),
    })
    .nullable(),
  is_bookable: z.boolean(),
  booking_url: z.string().nullable(),
  is_must_do: z.boolean(),
  is_hidden_gem: z.boolean(),
  sort_order: z.number(),
});
```

## TransitSegmentResponse

```typescript
const TransitSegmentResponse = z.object({
  mode: z.string(),
  duration_minutes: z.number(),
  distance_km: z.number().nullable(),
  cost_local: z.number().nullable(),
  cost_usd: z.number().nullable(),
  route_polyline: z.string().nullable(),
  notes: z.string().nullable(),
});
```

## DayResponse

```typescript
const DayResponse = z.object({
  id: z.string().uuid(),
  day_number: z.number(),
  date: z.string(),
  destination_city: z.string(),
  destination_country: z.string(),
  weather: z
    .object({
      summary: z.string(),
      icon: z.string(),
      temp_high_c: z.number(),
      temp_low_c: z.number(),
      rain_probability: z.number(),
    })
    .nullable(),
  estimated_spend_local: z.number().nullable(),
  estimated_spend_usd: z.number().nullable(),
  running_total_usd: z.number().nullable(),
  day_summary: z.string().nullable(),
  is_transit_day: z.boolean(),
  activities: z.array(ActivityResponse),
  transit_segments: z.array(TransitSegmentResponse),
});
```

## TripDetailResponse

```typescript
const TripDetailResponse = z.object({
  id: z.string().uuid(),
  slug: z.string(),
  title: z.string(),
  status: z.string(),
  is_public: z.boolean(),
  origin: z.object({
    city: z.string(),
    country: z.string(),
    lat: z.number(),
    lng: z.number(),
  }),
  destinations: z.array(
    z.object({
      city: z.string(),
      country: z.string(),
      lat: z.number(),
      lng: z.number(),
    }),
  ),
  start_date: z.string(),
  end_date: z.string(),
  duration_days: z.number(),
  group_type: z.string(),
  group_size: z.number(),
  budget_tier: z.string(),
  travel_pace: z.string(),
  vibes: z.array(z.string()),
  estimated_total_cost_usd: z.number().nullable(),
  local_currency: z.string().nullable(),
  must_do_status: z.array(
    z.object({
      name: z.string(),
      activity_id: z.string().nullable(),
      is_included: z.boolean(),
    }),
  ),
  days: z.array(DayResponse),
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
  created_at: z.string(),
});
```

## GenerationSSEEvent

SSE event types for streaming generation:

```typescript
const GenerationSSEEvent = z.discriminatedUnion("type", [
  z.object({ type: z.literal("status"), message: z.string() }),
  z.object({ type: z.literal("day"), day: DayResponse }),
  z.object({
    type: z.literal("must_dos"),
    must_dos: z.array(
      z.object({
        name: z.string(),
        emoji: z.string(),
        activity_id: z.string().nullable(),
        is_included: z.boolean(),
      }),
    ),
  }),
  z.object({
    type: z.literal("complete"),
    trip_id: z.string(),
    slug: z.string(),
    total_cost_usd: z.number(),
  }),
  z.object({ type: z.literal("error"), message: z.string() }),
]);
```
