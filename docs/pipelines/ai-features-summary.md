# AI Features Summary

| Feature                  | AI Model                  | Input                          | Output                    | Cost          |
| ------------------------ | ------------------------- | ------------------------------ | ------------------------- | ------------- |
| Itinerary generation     | Gemini 2.5 Flash          | Preferences + context          | Structured JSON itinerary | Free          |
| Interest tile generation | Gemini 2.5 Flash          | Destination + vibes            | 12-16 interest objects    | Free (cached) |
| Must-do generation       | Gemini 2.5 Flash          | Destination                    | 4-6 essential experiences | Free (cached) |
| Activity alternatives    | Gemini 2.5 Flash          | Current activity + constraints | 3-4 alternatives          | Free          |
| Day regeneration         | Gemini 2.5 Flash          | Day context + preferences      | New day JSON              | Free          |
| Lore quest generation    | Gemini 2.5 Flash          | Destination + culture data     | Quest objects             | Free (cached) |
| Photo verification       | Gemini 2.5 Flash Vision   | Photo + quest prompt           | Verification JSON         | Free          |
| Content moderation       | Google Cloud Vision       | Photo                          | SafeSearch scores         | Free (1K/mo)  |
| Semantic search          | Gemini text-embedding-004 | Query text                     | 768-dim vector            | Free          |
| Destination research     | Gemini 2.5 Flash          | Destination name               | Research JSON             | Free (cached) |
| Transport suggestions    | Gemini 2.5 Flash          | Origin + destination           | Transport options         | Free (cached) |
| Trip title generation    | Gemini 2.5 Flash          | Destinations + vibes           | Title string              | Free          |
| Pace adjustment          | Gemini 2.5 Flash          | Day activities + pace pref     | Redistributed activities  | Free          |
