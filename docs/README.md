# Drift API — Documentation

Comprehensive backend architecture documentation for the Drift travel platform API.

## Table of Contents

### Architecture

- [Flow Diagram](./flow-diagram.md) — Complete app flow with Mermaid diagrams
- [Technology Stack](./stack.md)
- [Project Structure](./project-structure.md)
- [Architecture Decisions](./decisions.md)

### Infrastructure

- [Middleware Stack](./middleware.md)
- [Redis Usage Plan](./redis.md)
- [Environment Variables](./environment.md)

### Database

- [Database Schema](./database/schema.md)
- [Migrations & Indexes](./database/migrations.md)

### API Endpoints

- [Auth](./endpoints/auth.md)
- [Destinations & Onboarding](./endpoints/destinations.md)
- [Trips](./endpoints/trips.md)
- [Itinerary Editing](./endpoints/itinerary.md)
- [Booking](./endpoints/booking.md)
- [Live Trip Companion](./endpoints/live.md)
- [Drift Lore & Gamification](./endpoints/lore.md)
- [Social & Community](./endpoints/social.md)
- [User Profiles](./endpoints/profiles.md)
- [Public Library](./endpoints/library.md)
- [Export & Share](./endpoints/export.md)
- [Admin & Moderation](./endpoints/admin.md)

### DTO Schemas (Zod)

- [Common Schemas](./dtos/common.md)
- [Auth DTOs](./dtos/auth.md)
- [Onboarding DTOs](./dtos/onboarding.md)
- [Trips & Itinerary DTOs](./dtos/trips.md)
- [Itinerary Editing DTOs](./dtos/itinerary.md)
- [Booking DTOs](./dtos/booking.md)
- [Lore & Gamification DTOs](./dtos/lore.md)
- [Social & Feed DTOs](./dtos/social.md)
- [Library DTOs](./dtos/library.md)
- [Profiles DTOs](./dtos/profiles.md)
- [Export DTOs](./dtos/export.md)

### AI Pipelines

- [Itinerary Generation Pipeline](./pipelines/itinerary-generation.md)
- [Image Recognition Pipeline](./pipelines/image-recognition.md)
- [Semantic Search (Embeddings)](./pipelines/semantic-search.md)
- [AI Features Summary](./pipelines/ai-features-summary.md)

### Implementation Phases

- [Phase 1: Foundation](./phases/phase-01-foundation.md)
- [Phase 2: Auth + Profiles](./phases/phase-02-auth-profiles.md)
- [Phase 3: Destinations & Onboarding](./phases/phase-03-destinations-onboarding.md)
- [Phase 4: AI Generation Engine](./phases/phase-04-ai-generation.md)
- [Phase 5: Itinerary Editing](./phases/phase-05-itinerary-editing.md)
- [Phase 6: Booking System](./phases/phase-06-booking.md)
- [Phase 7: Lore & Gamification](./phases/phase-07-lore-gamification.md)
- [Phase 8: Social & Feed](./phases/phase-08-social-feed.md)
- [Phase 9: Library & Search](./phases/phase-09-library-search.md)
- [Phase 10: Live Trip Companion](./phases/phase-10-live-companion.md)
- [Phase 11: Export & Share](./phases/phase-11-export-share.md)
- [Phase 12: Admin & Polish](./phases/phase-12-admin-polish.md)

### Testing

- [Verification Checklist](./verification.md)

---

**Total: ~95 endpoints across 12 route modules.**
