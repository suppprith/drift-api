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
  personalization_reason: z.string().nullable(), // why this was picked for this user
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

## TripMemberResponse

```typescript
const TripMemberResponse = z.object({
  id: z.string().uuid(),
  user: z.object({
    id: z.string().uuid(),
    username: z.string(),
    display_name: z.string(),
    avatar_url: z.string().nullable(),
    level_name: z.string(),
  }),
  role: z.enum(["owner", "editor", "viewer"]),
  status: z.enum(["pending", "accepted", "declined"]),
  invited_by: z.string().uuid().nullable(),
  joined_at: z.string().nullable(),
  created_at: z.string(),
});

const InviteToTripRequest = z.object({
  role: z.enum(["editor", "viewer"]).default("editor"),
  message: z.string().max(500).optional(), // optional personal message
});

const RespondToTripInvite = z.object({
  action: z.enum(["accept", "decline"]),
});

const UpdateMemberRoleRequest = z.object({
  role: z.enum(["editor", "viewer"]),
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

## TripHistoryItem

Lightweight representation of a completed trip for the history list.

```typescript
const TripHistoryItem = z.object({
  id: z.string().uuid(),
  slug: z.string(),
  title: z.string(),
  destinations: z.array(
    z.object({
      city: z.string(),
      country: z.string(),
    }),
  ),
  start_date: z.string(),
  end_date: z.string(),
  duration_days: z.number(),
  cover_photo_url: z.string().nullable(), // first trip photo or quest photo
  memory_summary: z.string().nullable(), // AI-generated one-liner
  memory_stats: z
    .object({
      activities_completed: z.number(),
      quests_completed: z.number(),
      photos_taken: z.number(),
      xp_earned: z.number(),
    })
    .nullable(),
  completed_at: z.string().nullable(),
  group_size: z.number(),
  members: z
    .array(
      z.object({
        username: z.string(),
        avatar_url: z.string().nullable(),
      }),
    )
    .optional(), // other trip members
});
```

## TripMemoryResponse

Full memory view of a completed trip — the nostalgia page.

```typescript
const TripMemoryResponse = z.object({
  trip: TripDetailResponse,
  memory_summary: z.string().nullable(), // AI-generated narrative summary
  memory_highlights: z.array(
    z.object({
      type: z.enum([
        "activity",
        "quest",
        "checkin",
        "challenge",
        "photo",
        "milestone",
      ]),
      title: z.string(),
      description: z.string(),
      photo_url: z.string().nullable(),
      activity_id: z.string().uuid().nullable(),
      day_number: z.number().nullable(),
      timestamp: z.string().nullable(),
    }),
  ),
  memory_stats: z.object({
    activities_completed: z.number(),
    activities_skipped: z.number(),
    total_activities: z.number(),
    quests_completed: z.number(),
    challenges_completed: z.number(),
    photos_taken: z.number(),
    checkins: z.number(),
    xp_earned: z.number(),
    total_spend_usd: z.number().nullable(),
    days_traveled: z.number(),
    top_category: z.string().nullable(), // most done activity category
  }),
  photos: z.array(TripPhotoResponse),
  feedback: z.array(ActivityFeedbackResponse),
  quest_completions: z.array(
    z.object({
      quest_title: z.string(),
      quest_emoji: z.string(),
      photo_url: z.string(),
      xp_granted: z.number(),
      completed_at: z.string(),
    }),
  ),
  challenge_results: z
    .array(
      z.object({
        title: z.string(),
        challenge_type: z.string(),
        your_progress: z.number(),
        target_count: z.number(),
        winner_username: z.string().nullable(),
        is_you_winner: z.boolean(),
      }),
    )
    .optional(),
});
```

## ActivityFeedbackRequest

```typescript
const ActivityFeedbackRequest = z.object({
  rating: z.enum(["loved", "liked", "neutral", "disliked"]),
  comment: z.string().max(500).optional(),
});
```

## ActivityFeedbackResponse

```typescript
const ActivityFeedbackResponse = z.object({
  id: z.string().uuid(),
  activity: z.object({
    id: z.string().uuid(),
    name: z.string(),
    category: z.string(),
    day_number: z.number(),
  }),
  rating: z.enum(["loved", "liked", "neutral", "disliked"]),
  comment: z.string().nullable(),
  was_skipped: z.boolean(),
  actual_duration_minutes: z.number().nullable(),
  created_at: z.string(),
});
```

## UploadTripPhotoRequest

```typescript
const UploadTripPhotoRequest = z.object({
  photo: z.string(), // base64 or S3 presigned upload key
  activity_id: z.string().uuid().optional(), // link to specific activity
  day_number: z.number().optional(), // link to specific day
  caption: z.string().max(500).optional(),
  lat: z.number().optional(),
  lng: z.number().optional(),
  taken_at: z.string().optional(), // ISO timestamp
});
```

## TripPhotoResponse

```typescript
const TripPhotoResponse = z.object({
  id: z.string().uuid(),
  photo_url: z.string(),
  caption: z.string().nullable(),
  lat: z.number().nullable(),
  lng: z.number().nullable(),
  taken_at: z.string().nullable(),
  activity: z
    .object({
      id: z.string().uuid(),
      name: z.string(),
    })
    .nullable(),
  day_number: z.number().nullable(),
  created_at: z.string(),
});
```
