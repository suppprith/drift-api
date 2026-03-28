# Export & Share Endpoints

> Phase 11: Export & Share

```
GET    /export/:tripId/pdf              → Generate PDF (returns S3 MinIO URL)
GET    /export/:tripId/share            → Get/create share link + OG metadata
GET    /export/:tripId/ical             → Export as iCal calendar file
GET    /export/:tripId/gpx              → Export map routes as GPX file
```
