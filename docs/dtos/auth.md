# Auth DTOs

> `routes/auth/schemas.ts`

Better Auth manages its own request/response types for sign-up, sign-in, OAuth, and session management. The DTOs below cover custom extensions (guest system, guest migration).

```typescript
const GuestSessionRequest = z.object({}); // no body needed

const GuestSessionResponse = z.object({
  guest_token: z.string().uuid(),
  expires_at: z.number(), // Unix timestamp, 72h from now
});

const MigrateGuestRequest = z.object({
  guest_token: z.string().uuid(),
});

const MigrateGuestResponse = z.object({
  migrated: z.boolean(),
  trip_id: z.string().uuid().nullable(),
});
```

## Better Auth Session (reference)

Better Auth's `getSession` returns:

```typescript
// From Better Auth — not custom-defined
{
  user: {
    id: string;
    name: string;
    email: string;
    emailVerified: boolean;
    image: string | null;
    createdAt: Date;
    updatedAt: Date;
  }
  session: {
    id: string;
    userId: string;
    token: string;
    expiresAt: Date;
  }
}
```
