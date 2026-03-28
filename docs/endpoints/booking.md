# Booking Endpoints

> Phase 5: Booking

```
GET    /trips/:id/booking-options       → Aggregated booking links for all bookable items
POST   /trips/:id/cart/add              → Add item to booking cart
DELETE /trips/:id/cart/:itemId          → Remove from cart
GET    /trips/:id/cart                   → Get current cart
PATCH  /trips/:id/cart/:itemId          → Update cart item (quantity, etc.)
POST   /trips/:id/cart/checkout         → Process checkout (affiliate tracking)
GET    /trips/:id/bookings              → Get confirmed bookings
```
