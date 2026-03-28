# Auth DTOs

> `routes/auth/schemas.ts`

```typescript
const SignupRequest = z.object({
  email: z.string().email().max(255),
  password: z.string().min(8).max(128),
  display_name: z.string().min(1).max(100),
  home_city: z.string().max(100).optional(),
});

const LoginRequest = z.object({
  email: z.string().email(),
  password: z.string(),
});

const AuthResponse = z.object({
  user: z.object({
    id: z.string().uuid(),
    email: z.string().email(),
    display_name: z.string(),
    avatar_url: z.string().nullable(),
    level: z.number(),
    level_name: z.string(),
    xp: z.number(),
  }),
  access_token: z.string(),
  refresh_token: z.string(),
  expires_at: z.number(),
});

const OAuthCallbackRequest = z.object({
  code: z.string(),
  redirect_uri: z.string().url(),
});
```
