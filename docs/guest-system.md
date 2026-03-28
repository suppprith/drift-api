# Guest System (Anonymous Access)

Users can go through onboarding and generate an itinerary **without signing up or logging in**. This reduces friction and lets users experience the core value before committing to an account.

---

## How It Works

1. User opens app → chooses **"Try without account"** or **"Skip for now"**
2. Backend creates a **guest session** → returns a `guest_token` (UUID)
3. Guest token is sent as `X-Guest-Token` header on all subsequent requests
4. Guest goes through full onboarding wizard → creates trip → generates itinerary
5. At any point, guest can sign up → guest data is **migrated** to their new account

---

## Guest Session Lifecycle

```
POST /auth/guest → { guest_token, expires_at }  (no body required)

                    ┌──────────────────────────────────┐
                    │  Redis: drift:guest:{token}       │
                    │  TTL: 72 hours                     │
                    │  Fields:                           │
                    │    created_at, ip, trip_id,        │
                    │    onboarding_state, generations   │
                    └──────────────────────────────────┘

After 72h with no sign-up → session expires, trip auto-deleted via QStash cleanup job
```

---

## What Guests CAN Do

| Feature                    | Access            |
| -------------------------- | ----------------- |
| Search destinations        | Yes               |
| Full onboarding wizard     | Yes               |
| Create a trip draft        | Yes (1 at a time) |
| Generate itinerary         | Yes (1 per day)   |
| View generated itinerary   | Yes               |
| Swap activities            | Yes (3 swaps max) |
| View trip by slug (public) | Yes               |
| Browse public library      | Yes               |
| Search library             | Yes               |

## What Guests CANNOT Do

| Feature                                   | Reason            |
| ----------------------------------------- | ----------------- |
| Save multiple trips                       | Sign up to save   |
| Edit itinerary (add/remove)               | Sign up to unlock |
| Regenerate full days                      | Sign up to unlock |
| Booking system                            | Requires account  |
| Live trip companion                       | Requires account  |
| Drift Lore / quests                       | Requires account  |
| Social features (upvote, comment, follow) | Requires account  |
| Export / share (PDF, link)                | Sign up to share  |
| Profile / gamification                    | Requires account  |
| Surprise me / adjust pace                 | Sign up to unlock |

---

## Guest-to-Account Migration

When a guest signs up or logs in, their session data is migrated:

```
POST /auth/signup   +  X-Guest-Token header
POST /auth/login    +  X-Guest-Token header
POST /auth/oauth/*  +  X-Guest-Token header
```

### Migration Steps

1. Validate `guest_token` → fetch guest session from Redis
2. Create account (or log in to existing)
3. Update `trips.user_id` from `NULL` → new user's UUID
4. Update `trips.guest_session_id` → `NULL`
5. Transfer onboarding state to user profile
6. Delete guest session from Redis
7. Return normal `AuthResponse` with JWT

---

## Database Changes

### trips table — new column

```sql
ALTER TABLE trips ADD COLUMN guest_session_id TEXT;
CREATE INDEX idx_trips_guest_session ON trips(guest_session_id) WHERE guest_session_id IS NOT NULL;
```

`user_id` is already nullable (`REFERENCES profiles(id) ON DELETE SET NULL`), so guest trips set `user_id = NULL` and use `guest_session_id` instead.

### guest_sessions table (optional — Redis-primary)

Guest sessions live in Redis. No dedicated DB table needed. The `guest_session_id` on the `trips` table is the only DB footprint.

---

## Redis Keys

```
drift:guest:{token}                → HASH: guest session state
drift:guest:{token}:trip           → STRING: trip UUID (max 1)
drift:guest:{token}:gen_count      → STRING: generation count today (INCR + EXPIRE 24h)
drift:guest:{token}:swap_count     → STRING: swap count for current trip (INCR)
drift:rate:{token}:api             → STRING: API rate limit counter (same as anonymous)
drift:rate:{token}:gen             → STRING: generation rate limit (1/day)
```

### Guest Session Hash Fields

```
HSET drift:guest:{token}
  created_at      "2026-03-29T10:00:00Z"
  ip              "203.0.113.42"
  trip_id         "uuid-of-trip"
  onboarding      "{...serialized onboarding state...}"
  generations     "0"
```

TTL: **72 hours** — refreshed on each activity.

---

## Rate Limiting for Guests

| Limit               | Value                               |
| ------------------- | ----------------------------------- |
| API requests/minute | 60 (same as anonymous)              |
| Generations/day     | 1                                   |
| Active trips        | 1                                   |
| Swaps per trip      | 3                                   |
| Onboarding sessions | 3 per IP per day (abuse prevention) |

---

## Middleware: Guest Auth

New middleware that runs on **public** routes (not under `/api/protected/`):

```typescript
// Pseudo-code
const guestAuth = () => async (c, next) => {
  const guestToken = c.req.header("X-Guest-Token");
  if (guestToken) {
    const session = await redis.hgetall(`drift:guest:${guestToken}`);
    if (session) {
      c.set("guestSession", session);
      c.set("guestToken", guestToken);
      // Refresh TTL on activity
      await redis.expire(`drift:guest:${guestToken}`, 72 * 60 * 60);
    }
  }
  await next();
};
```

### Route Access Pattern

```
/api/guest/*          → Guest-accessible routes (guest OR authenticated)
/api/protected/*      → Authenticated-only routes (JWT required)
/api/*                → Fully public routes (no auth needed)
```

Guest routes accept **either** a valid JWT **or** a valid `X-Guest-Token`. If neither is present, return `401`.

---

## Cleanup (QStash Cron)

Daily cron job to clean up expired guest data:

```
POST /admin/cleanup/guest-sessions (QStash scheduled, daily)
```

1. Query `trips WHERE guest_session_id IS NOT NULL AND created_at < NOW() - INTERVAL '3 days'`
2. Delete orphaned trips + itinerary_days + itinerary_activities (CASCADE)
3. Log cleanup stats

---

## UX Prompts to Convert

At key moments, nudge guests to sign up:

| Trigger                         | Prompt                                                                             |
| ------------------------------- | ---------------------------------------------------------------------------------- |
| After itinerary generated       | "Sign up to save this trip and unlock editing"                                     |
| Trying to swap 4th activity     | "You've used all 3 free swaps. Sign up for unlimited swaps!"                       |
| Trying to access locked feature | "Create a free account to unlock [feature]"                                        |
| After 24h with no sign-up       | Push notification (if permitted): "Your trip expires in 48h — sign up to keep it!" |
| Viewing generated itinerary     | Persistent banner: "This trip will expire. Sign up to save it forever."            |

---

## Implementation Phase

This should be implemented in **Phase 2 (Auth + Profiles)** alongside the main auth system, since guest sessions are part of the authentication flow.
