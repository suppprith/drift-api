# Public Library Endpoints

> Phase 10: Public Library

```
GET    /library                          → Browse public itineraries (paginated, filtered)
GET    /library/search?q=               → Full-text search itineraries
GET    /library/trending                 → Trending itineraries
GET    /library/destinations             → Popular destinations with trip counts
POST   /library/clone/:tripId           → Fork a public trip as template
GET    /library/sitemap                  → Sitemap XML for SEO
```
