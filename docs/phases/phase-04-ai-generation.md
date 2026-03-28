# Phase 4: AI Generation Engine (Week 2-3)

_Depends on Phase 3_

## Tasks

1. Gemini client wrapper (text + streaming)
2. System prompt engineering (local-first rules)
3. Itinerary generation prompt template
4. SSE streaming endpoint for generation
5. Post-processing: geocode validation, cost totals
6. Trip CRUD endpoints
7. Save generated itinerary to Supabase
8. Redis caching of generated trips

## Relevant Files

- `src/routes/trips/*`
- `src/services/ai/gemini.ts`
- `src/services/ai/prompts/*.ts`
- `src/services/ai/streaming.ts`
- `src/services/data/trips.ts`
- `src/services/data/itineraries.ts`

## Related Docs

- [Trips Endpoints](../endpoints/trips.md)
- [Trips DTOs](../dtos/trips.md)
- [Itinerary Generation Pipeline](../pipelines/itinerary-generation.md)
- [AI Features Summary](../pipelines/ai-features-summary.md)
