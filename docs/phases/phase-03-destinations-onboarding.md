# Phase 3: Destinations & Onboarding (Week 2)

## Tasks

1. Mapbox geocoding proxy endpoint
2. Destination cache service (Redis → Supabase → Gemini)
3. Interest tile generation endpoint
4. Must-do generation endpoint
5. Weather service (Open-Meteo client)
6. Currency service (ExchangeRate-API client)
7. Reddit intel service (OAuth + fetch + extract)
8. Background prefetch endpoint (fire-and-forget)
9. Transport options endpoint

## Relevant Files

- `src/routes/destinations/*`
- `src/routes/onboarding/*`
- `src/services/external/mapbox.ts`
- `src/services/external/weather.ts`
- `src/services/external/currency.ts`
- `src/services/external/reddit.ts`
- `src/services/cache/layers.ts`

## Related Docs

- [Destinations Endpoints](../endpoints/destinations.md)
- [Onboarding DTOs](../dtos/onboarding.md)
- [Redis Usage](../redis.md)
