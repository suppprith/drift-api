# Destinations & Onboarding Endpoints

> Phase 2: Onboarding

## Destination Search & Intel

```
GET    /destinations/search?q=          → Mapbox geocoding autocomplete proxy
GET    /destinations/:key               → Get cached destination data
GET    /destinations/:key/interests     → AI-generated interest tiles (cached)
GET    /destinations/:key/must-dos      → Must-do experiences (cached)
GET    /destinations/:key/transport?origin= → Transport options from origin
POST   /destinations/:key/prefetch      → Fire-and-forget: trigger Reddit + AI research
```

## Weather

```
GET    /weather?lat=&lng=&start=&end=   → Weather data for date range
```

## Currency

```
GET    /currency/rates?base=&targets=   → Exchange rates (cached daily)
```
