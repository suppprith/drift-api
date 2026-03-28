# Common Schemas

> `schemas/common.ts`

```typescript
const PaginationQuery = z.object({
  page: z.coerce.number().int().min(1).default(1),
  limit: z.coerce.number().int().min(1).max(100).default(20),
  sort_by: z.string().optional(),
  sort_order: z.enum(["asc", "desc"]).default("desc"),
});

const PaginatedResponse = <T extends z.ZodType>(itemSchema: T) =>
  z.object({
    items: z.array(itemSchema),
    total: z.number(),
    page: z.number(),
    limit: z.number(),
    has_more: z.boolean(),
  });

const ErrorResponse = z.object({
  error: z.object({
    code: z.string(),
    message: z.string(),
    details: z.any().optional(),
  }),
});

const SuccessResponse = z.object({
  success: z.boolean(),
  message: z.string().optional(),
});

const CoordinatesSchema = z.object({
  lat: z.number().min(-90).max(90),
  lng: z.number().min(-180).max(180),
});
```
