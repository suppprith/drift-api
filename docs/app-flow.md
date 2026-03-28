# Drift — How the App Works

A plain-language walkthrough of every major feature and how users experience the app from start to finish.

---

## 1. Opening the App

When a user opens Drift for the first time, they see two options:

- **Sign up / Log in** — Create an account with email, Google, or Apple
- **Try without an account** — Jump straight in as a guest

Guests can do the core thing — plan a trip and get an AI-generated itinerary — without creating an account. They're nudged to sign up at key moments so they don't lose their trip.

---

## 2. Guest Mode

If the user chooses to skip sign-up:

1. The app creates a temporary guest session (lasts 72 hours)
2. The guest can search destinations, go through onboarding, and generate **one itinerary per day**
3. They can swap up to **3 activities** in the generated plan
4. They **cannot** save multiple trips, edit the itinerary fully, use booking, go on live trips, do quests, or use social features
5. At any point, they can sign up — and the trip they created transfers to their new account automatically
6. If they don't sign up within 72 hours, the guest session and trip are cleaned up

---

## 3. Signing Up / Logging In

Users can create an account using:

- **Email + password**
- **Google OAuth**
- **Apple OAuth**

After signing up, they get a secure token that keeps them logged in. If they were a guest before, their trip is automatically linked to their new account — nothing is lost.

They can also reset their password via email or delete their account entirely (all data is wiped for privacy compliance).

---

## 4. Home Screen

Once logged in, the user lands on the home screen with access to:

| Section             | What it does                                   |
| ------------------- | ---------------------------------------------- |
| **Create New Trip** | Start planning a new trip                      |
| **My Trips**        | View all past and upcoming trips               |
| **Browse Library**  | Explore public itineraries made by other users |
| **Social Feed**     | See what other travelers are posting           |
| **My Profile**      | View stats, achievements, saved trips          |
| **Drift Lore**      | Explore quests and challenges at destinations  |

Guests only see "Create New Trip" and "Browse Library" — the rest requires an account.

---

## 5. Planning a Trip (Onboarding Wizard)

When the user taps "Create New Trip," they go through a step-by-step wizard:

### Step 1: Where are you going?

- Type a destination and pick from autocomplete suggestions (powered by Mapbox)
- As soon as they pick a place, the app starts researching that destination in the background — pulling Reddit tips, AI-generated local knowledge, and more

### Step 2: When?

- Pick start and end dates
- The app fetches weather forecasts and local currency exchange rates for those dates

### Step 3: How are you getting there?

- Choose transport: flight, train, bus, drive, ferry
- See estimated travel time, price range, and carbon footprint for each option

### Step 4: Tell us about your group

- Solo, couple, friends, or family?
- Group size, kids, budget tier (budget / mid-range / luxury)
- Travel pace (slow / balanced / packed)
- Getting around preference (walking / mix / taxis)
- Food preferences and allergies
- Accommodation style (hostel to luxury resort)
- Accessibility needs

### Step 5: What are you into?

- Pick up to 6 vibes: adventure, history, food, nightlife, spiritual, art, nature, photography, shopping, wellness, music, offbeat
- See destination-specific interest tiles and must-do activities
- Select specific must-dos to prioritize

### Step 6: Review & Generate

- See a summary of all choices
- Hit "Generate" to create the itinerary

---

## 6. AI Itinerary Generation

This is the core magic. When the user hits generate:

1. The app saves a trip draft with all their preferences
2. AI (Gemini 2.5 Flash) receives everything — destination research, Reddit intel, weather data, exchange rates, the user's preferences, must-dos, and interests
3. The itinerary streams back in real time (the user sees it building day by day)
4. Each day includes:
   - **Activities** with times, descriptions, addresses, and costs in local currency + USD
   - **Local tips** — insider advice like "go before 9am to avoid crowds"
   - **Food recommendations** with specific dish suggestions
   - **Hidden gems** — offbeat spots most tourists miss
   - **Rain backup plans** — alternative activities if weather turns bad
   - **Tourist trap warnings** — places to skip, with better local alternatives
   - **Running cost tracker** — see spending build up day by day
5. After streaming, each location is validated on the map, enriched with TripAdvisor ratings, and transit routes are calculated between stops
6. Must-do items are cross-referenced to make sure they're included

**For guests:** Limited to 1 generation per day. For free accounts: 5/day. Pro: unlimited.

---

## 7. Viewing & Editing the Itinerary

Once generated, users see a full day-by-day itinerary. Logged-in users can customize it:

| Action                           | What happens                                                       |
| -------------------------------- | ------------------------------------------------------------------ |
| **Swap an activity**             | AI suggests 3–4 alternatives to pick from                          |
| **Add an activity**              | Manually add something to any day                                  |
| **Remove an activity**           | Delete something you don't want                                    |
| **Reorder activities**           | Drag to rearrange the day's schedule                               |
| **Move activity to another day** | Shift something to a different day                                 |
| **Regenerate a day**             | AI rebuilds that entire day from scratch                           |
| **"Surprise me"**                | AI adds hidden gems to each day                                    |
| **Adjust pace**                  | AI redistributes activities if a day feels too packed or too light |
| **Add/remove days**              | Extend or shorten the trip                                         |

**For guests:** Can only swap (up to 3 times). All other edits require an account.

---

## 8. Booking

From the itinerary, logged-in users can book things directly:

1. See booking options for hotels, tours, tickets, and transport
2. Add items to a cart
3. Review the cart with total costs
4. "Checkout" redirects to provider websites via affiliate links (the app doesn't handle payments directly)
5. Confirmed bookings are tracked on the trip

---

## 9. Live Trip Companion

When the trip date arrives, logged-in users can activate the live companion:

- **Real-time tracking** — See where you are on today's itinerary
- **Auto check-in** — The app detects when you arrive at a location (GPS geofencing) and checks you in automatically
- **Running late?** — Tell the app you're behind schedule, and AI recalculates the rest of your day
- **Skip an activity** — Mark something as skipped, and the schedule adjusts
- **Nearby suggestions** — See what's close to your current location
- **Group sync** — If traveling with friends who also have the app, everyone's progress syncs in real time

---

## 10. Drift Lore (Quests & Gamification)

This is the game layer on top of travel. Logged-in users can:

### Quests

- Browse quests at their destination — things like "Find the hidden street art in lane 4" or "Eat at the oldest cafe in the city"
- Quests appear on a map and can be filtered by type
- Accept a quest, go do it, then complete it by uploading a photo as proof

### Photo Verification

When a user submits a quest photo, the app:

1. Checks the photo's GPS data to confirm they were actually there
2. Scans for inappropriate content
3. Uses AI vision to verify the photo matches the quest (e.g., "Is this actually the temple?")
4. Awards XP if everything checks out

### XP & Leveling

- Every quest completion, check-in, and trip earns XP
- XP accumulates toward levels with themed names (Wanderer → Explorer → Pathfinder → etc.)
- Achievements unlock for milestones like "Visit 5 countries" or "Complete 10 quests"

### Leaderboards

- Global leaderboard ranked by XP
- Per-destination leaderboards — compete with others who visited the same place

### Rewards

- Earn rewards like discounts on bookings
- Redeem rewards from the profile

---

## 11. Social & Community

Logged-in users can engage with the community:

- **Social feed** — See trips shared by other users, filtered by global, destination-specific, following, or trending
- **Upvote** trips and quest completions
- **Save** trips you like for later
- **Comment** on trips
- **Follow** other travelers
- **Report** inappropriate content

---

## 12. Public Library

Anyone (including guests) can browse the public itinerary library:

- **Browse** — See all public itineraries
- **Search** — Natural language search ("3 days in Tokyo on a budget with kids") powered by AI semantic search
- **Trending** — See what's popular right now
- **Destinations** — Browse by destination
- **Clone** — Fork any public itinerary as a starting point for your own trip, then customize it

---

## 13. Export & Share

Logged-in users can export their itinerary in multiple formats:

- **PDF** — Beautiful printable itinerary
- **iCal** — Import into Google Calendar, Apple Calendar, etc.
- **GPX** — Import routes into maps apps
- **Share link** — Shareable URL with preview image and metadata

---

## 14. User Profile

Every account has a profile:

- **Public profile** — Username, avatar, level, trip count, countries visited
- **My trips** — All created trips
- **Saved trips** — Trips saved from the library or feed
- **Stats** — Countries, trips, quests completed, XP breakdown
- **Notifications** — Level-ups, quest completions, social interactions, system messages
- **Settings** — Update profile, avatar, preferences

---

## 15. Admin & Moderation

Behind the scenes, admins can:

- Review reported content and take action (warn, remove, ban)
- Moderate photos that were flagged by the AI as uncertain
- View platform-wide stats (users, trips, generations)
- Create and manage quests
- Invalidate cached data when needed
- Ban/unban users

---

## The Big Picture

```
Guest opens app
    │
    ├── Try without account ──→ Onboarding ──→ Generate itinerary ──→ View trip
    │                                                                      │
    │                                                                Sign up prompt
    │                                                                      │
    └── Sign up / Log in ──→ Home ──→ Create trip ──→ Onboarding ──→ Generate
                              │                                          │
                              │                                     View & Edit
                              │                                          │
                              │                              ┌───────────┼───────────┐
                              │                              │           │           │
                              │                           Booking   Export/Share  Live Companion
                              │
                              ├── My Trips ──→ View past trips
                              ├── Library ──→ Browse / Search / Clone
                              ├── Social Feed ──→ Upvote / Comment / Follow
                              ├── Drift Lore ──→ Quests ──→ Photo proof ──→ XP / Rewards
                              └── Profile ──→ Stats / Achievements / Leaderboards
```

---

## Key Design Principles

1. **No login wall for the core experience** — Anyone can plan a trip and see an AI itinerary without an account
2. **AI does the research** — Users don't need to spend hours on travel blogs; the AI aggregates local knowledge, Reddit tips, weather, costs, and ratings
3. **Every suggestion is local-first** — Tourist traps are flagged, local alternatives are suggested, hidden gems are highlighted
4. **Travel is a game** — XP, levels, quests, and leaderboards make exploring feel rewarding
5. **Community-powered** — Public trips, social feed, and semantic search mean everyone benefits from everyone else's travels
6. **Real-time companion** — The app doesn't stop being useful once the trip starts; it guides you through each day
