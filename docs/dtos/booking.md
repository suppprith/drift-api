# Booking DTOs

> `routes/booking/schemas.ts`

```typescript
const AddToCartRequest = z.object({
  item_type: z.enum([
    "flight",
    "hotel",
    "experience",
    "restaurant",
    "transport",
    "insurance",
  ]),
  activity_id: z.string().uuid().optional(),
  title: z.string(),
  description: z.string().optional(),
  date: z.string().date().optional(),
  quantity: z.number().int().min(1).default(1),
  unit_price_usd: z.number(),
  unit_price_local: z.number().optional(),
  currency: z.string().optional(),
  booking_url: z.string().url(),
  provider: z.string(),
});

const CartResponse = z.object({
  items: z.array(
    z.object({
      id: z.string().uuid(),
      item_type: z.string(),
      title: z.string(),
      description: z.string().nullable(),
      date: z.string().nullable(),
      quantity: z.number(),
      unit_price_usd: z.number(),
      subtotal_usd: z.number(),
      provider: z.string(),
      booking_url: z.string(),
      status: z.string(),
    }),
  ),
  total_usd: z.number(),
  total_local: z.number().nullable(),
  local_currency: z.string().nullable(),
  item_count: z.number(),
});

const CheckoutRequest = z.object({
  item_ids: z.array(z.string().uuid()).optional(), // specific items, or all if omitted
});

const CheckoutResponse = z.object({
  booking_references: z.array(
    z.object({
      item_id: z.string().uuid(),
      provider: z.string(),
      booking_url: z.string(), // affiliate-tagged URL
      tracking_id: z.string(),
    }),
  ),
  total_usd: z.number(),
});
```
