# Redis (Upstash) Usage Plan

## Cache Key Naming Convention

```
drift:{scope}:{entity}:{id}:{sub}
```

### Examples

```
drift:dest:pokhara-nepal:research         → destination research JSON
drift:dest:pokhara-nepal:interests        → interest tiles JSON
drift:dest:pokhara-nepal:must-dos         → must-dos JSON
drift:dest:pokhara-nepal:reddit           → Reddit intel JSON
drift:weather:28.2-83.9:2026-03-29       → weather data
drift:currency:USD:2026-03-29            → exchange rates
drift:trip:{tripId}                       → cached trip response
drift:trip:{tripId}:cart                  → booking cart
drift:rate:{userId}:gen                   → generation rate limit counter
drift:rate:{ip}:api                       → API rate limit counter
drift:session:{sessionId}:onboarding     → onboarding wizard state
drift:leaderboard:global                  → global XP sorted set
drift:leaderboard:dest:pokhara-nepal     → destination leaderboard
drift:lock:gen:{tripId}                   → distributed lock for generation
drift:feed:trending                       → trending feed cache
drift:user:{userId}:notifications:unread → unread notification count
```

---

## Redis Data Structures Used

| Structure     | Use Case                                          | Commands                       |
| ------------- | ------------------------------------------------- | ------------------------------ |
| STRING        | Destination cache, weather, rates, trip responses | GET, SET, SETEX                |
| HASH          | Onboarding wizard state (multi-field)             | HSET, HGET, HGETALL            |
| SORTED SET    | Leaderboards (XP score → user)                    | ZADD, ZRANGE, ZRANK, ZREVRANGE |
| LIST          | Generation queue (FIFO)                           | LPUSH, RPOP, LLEN              |
| SET           | User's completed quest IDs (fast lookup)          | SADD, SISMEMBER, SCARD         |
| STRING + NX   | Distributed locks                                 | SET key val NX EX 30           |
| STRING + INCR | Rate limiting counters                            | INCR, EXPIRE, GET              |

---

## TTL Strategy

| Cache Type           | TTL                     | Reason               |
| -------------------- | ----------------------- | -------------------- |
| Destination research | 30 days                 | Changes slowly       |
| Reddit intel         | 7 days                  | Refreshed weekly     |
| Interest tiles       | 7 days                  | Relatively stable    |
| Must-dos             | 30 days                 | Rarely changes       |
| Weather forecast     | 6 hours                 | Updates frequently   |
| Weather historical   | 30 days                 | Doesn't change       |
| Exchange rates       | 24 hours                | Daily updates        |
| Trip response        | 1 hour                  | User might edit      |
| API rate limits      | 1 minute sliding window | Short-lived          |
| Gen rate limits      | 24 hours                | Daily quota          |
| Leaderboards         | 5 minutes               | Near-real-time       |
| Onboarding state     | 24 hours                | Session-bound        |
| Trending feed        | 5 minutes               | Frequently refreshed |
