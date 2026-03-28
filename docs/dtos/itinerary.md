# Itinerary Editing DTOs

> `routes/itinerary/schemas.ts`

```typescript
const UpdateActivityRequest = z.object({
  start_time: z
    .string()
    .regex(/^\d{2}:\d{2}$/)
    .optional(),
  end_time: z
    .string()
    .regex(/^\d{2}:\d{2}$/)
    .optional(),
  name: z.string().max(200).optional(),
  description: z.string().max(2000).optional(),
  notes: z.string().max(500).optional(),
});

const AddActivityRequest = z.object({
  start_time: z.string().regex(/^\d{2}:\d{2}$/),
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
  name: z.string().max(200),
  description: z.string().max(2000).optional(),
  address: z.string().max(500).optional(),
  lat: z.number().optional(),
  lng: z.number().optional(),
  cost_local: z.number().optional(),
  duration_minutes: z.number().int().optional(),
  insert_after: z.string().uuid().optional(), // activity ID to insert after
});

const ReorderActivitiesRequest = z.object({
  activity_ids: z.array(z.string().uuid()), // new order
});

const SwapAlternativesResponse = z.object({
  current: ActivityResponse,
  alternatives: z
    .array(
      ActivityResponse.extend({
        swap_reason: z.string(),
      }),
    )
    .min(3)
    .max(5),
});

const ApplySwapRequest = z.object({
  activity_id: z.string().uuid(), // current activity to replace
  alternative_index: z.number().int(), // which alternative to apply
});
```
