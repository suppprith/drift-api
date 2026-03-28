# Phase 7: Lore & Gamification (Week 4-5)

## Tasks

1. Quest endpoints (list, nearby, active for trip)
2. Photo upload to S3 MinIO
3. Image recognition pipeline (Gemini Vision + Cloud Vision)
4. Quest completion submission endpoint
5. XP/level calculation service
6. Leaderboard endpoints (Redis sorted sets)
7. Achievement system
8. Reward engine (discount generation)
9. Notification service

## Relevant Files

- `src/routes/lore/*`
- `src/services/ai/imageRecognition.ts`
- `src/services/ai/prompts/photoVerify.ts`
- `src/services/external/cloudVision.ts`
- `src/services/gamification/*.ts`
- `src/services/notifications/*.ts`

## Related Docs

- [Lore Endpoints](../endpoints/lore.md)
- [Lore DTOs](../dtos/lore.md)
- [Image Recognition Pipeline](../pipelines/image-recognition.md)
