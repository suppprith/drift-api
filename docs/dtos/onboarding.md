# Onboarding DTOs

> `routes/onboarding/schemas.ts`

```typescript
const DestinationSearchQuery = z.object({
  q: z.string().min(2).max(200),
  types: z.string().optional(), // 'place,locality,neighborhood'
  limit: z.coerce.number().int().min(1).max(10).default(5),
});

const DestinationSearchResult = z.object({
  place_name: z.string(),
  city: z.string(),
  country: z.string(),
  lat: z.number(),
  lng: z.number(),
  mapbox_id: z.string(),
});

const TransportOption = z.object({
  mode: z.enum([
    "direct_flight",
    "connecting_flight",
    "train",
    "bus",
    "overnight_bus",
    "ferry",
    "drive",
  ]),
  icon: z.string(),
  estimated_duration: z.string(), // "2h 30m"
  estimated_price_range: z.object({
    min_usd: z.number(),
    max_usd: z.number(),
    min_local: z.number(),
    max_local: z.number(),
    currency: z.string(),
  }),
  carbon_footprint_kg: z.number(),
  notes: z.string().optional(), // "Overnight, departs 8pm"
});

const TravelerProfile = z.object({
  group_type: z.enum(["solo", "couple", "friends", "family"]),
  group_size: z.number().int().min(1).max(20),
  has_kids: z.boolean().default(false),
  budget_tier: z.enum(["budget", "mid-range", "luxury"]),
  travel_pace: z.enum(["slow", "balanced", "packed"]),
  getting_around: z.enum(["walk", "mix", "taxi"]),
  food_preferences: z.array(
    z.enum([
      "veg",
      "eats_everything",
      "halal",
      "vegan",
      "no_spicy",
      "seafood_lover",
      "street_food",
    ]),
  ),
  food_allergies: z.string().max(500).optional(),
  accommodation_type: z.enum([
    "hostel_dorm",
    "private_hostel",
    "guesthouse",
    "mid_hotel",
    "boutique_hotel",
    "luxury_resort",
    "airbnb",
  ]),
  vibes: z
    .array(
      z.enum([
        "adventure",
        "history",
        "food",
        "nightlife",
        "spiritual",
        "art",
        "nature",
        "photography",
        "shopping",
        "wellness",
        "music",
        "offbeat",
      ]),
    )
    .min(1)
    .max(6),
  accessibility_needed: z.boolean().default(false),
  accessibility_notes: z.string().max(500).optional(),
  specific_requests: z.string().max(1000).optional(),
});

const CreateTripRequest = z.object({
  origin: z.object({
    city: z.string(),
    country: z.string(),
    lat: z.number(),
    lng: z.number(),
  }),
  destinations: z
    .array(
      z.object({
        city: z.string(),
        country: z.string(),
        lat: z.number(),
        lng: z.number(),
        days_allocated: z.number().int().min(1).optional(),
      }),
    )
    .min(1)
    .max(5),
  start_date: z.string().date(),
  end_date: z.string().date(),
  transport_mode: z.string(),
  profile: TravelerProfile,
  selected_interests: z.array(z.string()).min(1).max(20),
});
```
