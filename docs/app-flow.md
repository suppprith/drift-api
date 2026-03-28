# Drift — How the App Works

A plain-language walkthrough of every major feature and how users experience the app from start to finish.

---

## 1. Opening the App

When a user opens Drift for the first time, they see two options:

- **Sign up / Log in** — Create an account with email or Google
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

This is the core magic — and every itinerary is **deeply personalized**. No two users get the same trip, even for the same destination.

When the user hits generate:

1. The app saves a trip draft with all their preferences
2. AI receives **two layers of context**:
   - **Destination context** (shared): destination research, Reddit intel, weather, exchange rates
   - **User-specific context** (unique to this user): their travel DNA, past trip history, activity feedback, previously visited places, quest patterns, and behavioral signals
3. The itinerary streams back in real time (the user sees it building day by day)
4. Each day includes:
   - **Activities** with times, descriptions, addresses, and costs in local currency + USD
   - **Personalization reasons** — a note explaining why this activity was picked for _this_ user (e.g., "Based on your love for street food" or "You haven't tried a traditional cooking class yet")
   - **Local tips** — insider advice like "go before 9am to avoid crowds"
   - **Food recommendations** with specific dish suggestions matched to the user's food adventurousness
   - **Hidden gems** — offbeat spots most tourists miss, weighted by user's hidden gem affinity
   - **Rain backup plans** — alternative activities if weather turns bad
   - **Tourist trap warnings** — places to skip, with better local alternatives
   - **Running cost tracker** — see spending build up day by day
5. After streaming, each location is validated on the map, enriched with TripAdvisor ratings, and transit routes are calculated between stops
6. **Repeat filter** — the AI never suggests places the user has already visited or checked into on past trips
7. Must-do items are cross-referenced to make sure they're included

### What Makes It Personal

The more you use Drift, the better it gets at knowing what you want:

| If you...                                           | The AI learns to...                                        |
| --------------------------------------------------- | ---------------------------------------------------------- |
| Consistently skip nightlife activities              | Stop scheduling late-night plans                           |
| Rate food experiences as "loved"                    | Include more and better food spots                         |
| Complete lots of adventure quests                   | Suggest more adventurous, off-the-beaten-path activities   |
| Usually check in early in the morning               | Schedule your best activities in the morning               |
| Leave art museums quickly (short check-in duration) | Suggest fewer museum stops                                 |
| Travel at a slow pace across multiple trips         | Give you fewer activities per day with more breathing room |

This is powered by your **Travel DNA** — a profile that evolves after every completed trip.

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

## 8. Trip Collaboration

Trips aren't solo-only — users can plan and travel together:

### Inviting Friends

- From any trip you own, **invite friends** to join
- Choose their role: **editor** (can modify the itinerary) or **viewer** (read-only)
- Friends get a notification and can accept or decline
- The trip shows up in their "My Trips" once accepted

### Requesting to Join

- If a friend has a trip you want in on, **request to join**
- The trip owner gets a notification and can accept or decline
- Once accepted, you're added as a member

### How Shared Trips Work

- All members see the same itinerary in real time
- **Editors** can swap activities, add/remove items, adjust pace — just like the owner
- **Viewers** can see the full plan but can't change anything
- The owner can change roles or remove members at any time
- Everyone's edits show up for the whole group

---

## 9. Booking

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

## 10. Activity Feedback & Trip Photos

During and after a trip, users can capture their experience:

### Rating Activities

- After visiting an activity (or at any point), **rate it**: loved, liked, neutral, or disliked
- Optionally add a short comment ("The view was incredible" or "Way too crowded, not worth it")
- Ratings are used by AI to personalize future trips — this is the main feedback loop
- Activities you skip during the live companion are automatically logged as soft negative signals

### Trip Photos

- **Upload photos** from your trip — link them to specific activities or days
- Photos are stored permanently and appear in your trip memories
- These are separate from quest photos — just regular trip memories
- No photo verification needed, just basic content moderation

---

## 11. Trip History & Memories

Once a trip is completed, it becomes a **memory** — a nostalgic, rich look back at what you experienced.

### Completing a Trip

- When the trip dates pass (or the user manually marks it complete), the app:
  1. Collects all check-ins, skipped activities, quest completions, photos, challenge results, and feedback
  2. AI generates a **memory summary** — a short narrative about the trip ("Your 5 days in Kyoto were full of hidden temples, incredible ramen, and that mythic quest at Fushimi Inari...")
  3. AI picks **highlights** — the best moments based on check-in duration, ratings, and photos
  4. Stats are calculated: activities done vs skipped, quests completed, XP earned, money spent, challenges won
  5. The user's **Travel DNA** is updated based on all the signals from this trip

### Viewing Memories

- Go to **My Trips → Completed** to see all past trips
- Each trip card shows: destinations, dates, a cover photo, the memory summary one-liner, and key stats
- Tap into a trip to see the **full memory view**:

| Section        | What's included                                                                  |
| -------------- | -------------------------------------------------------------------------------- |
| **Summary**    | AI-generated narrative of the trip                                               |
| **Highlights** | Best moments — activities you loved, quest photos, milestones                    |
| **Day-by-Day** | The original itinerary with what you actually did (check-ins, skips, time spent) |
| **Photos**     | All trip photos + quest completion photos, mapped to locations                   |
| **Quests**     | Quests completed during this trip with proof photos and XP                       |
| **Challenges** | Group challenge results (if it was a shared trip)                                |
| **Stats**      | Activities completed, quests done, XP earned, total spend, top category          |
| **Feedback**   | Your ratings on activities — helps you remember what was worth it                |

### Why It Matters

- Memories are permanent — they never expire or get deleted
- They feed the personalization engine: your next trip is better because of what you loved (or didn't) on past trips
- You can share completed trips to the social feed for others to see
- It turns Drift from a planning tool into a **travel journal**

---

## 12. Drift Lore (Quests & Gamification)

This is the game layer on top of travel. Logged-in users can:

### Solo Quests

- Browse quests at their destination — things like "Find the hidden street art in lane 4" or "Eat at the oldest cafe in the city"
- Quests appear on a map and can be filtered by type
- Accept a quest, go do it, then complete it by uploading a photo as proof

### Group Side-Quests

- On shared trips, special **group quests** appear that require multiple people
- Examples: "Everyone take a photo at the same landmark" or "Your group finds 3 hidden alleys"
- Group quests grant bonus XP to all participants
- Progress is tracked per-member and visible to the whole group

### Group Side-Quests

- On shared trips, special **group quests** appear that require multiple people
- Examples: "Everyone take a photo at the same landmark" or "Your group finds 3 hidden alleys"
- Group quests grant bonus XP to all participants
- Progress is tracked per-member and visible to the whole group

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

## 13. Friends

Drift has a proper friends system (not just follow/unfollow):

- **Send a friend request** to any user from their profile
- **Accept or decline** incoming requests
- **Mutual friends** — See who you have in common with another user
- **Friends list** — View all your friends, their level, trip count
- **Block** — Block someone to prevent all interaction
- **Invite to trips** — Add friends as editors or viewers on your trips
- **Group challenges** — Compete with friends during shared trips
- Friends see each other's trips in their feed first
- During live trips, friends traveling together get real-time group sync
- Friend activity shows up in notifications ("Your friend just completed a mythic quest in Kyoto!")

---

## 14. Social & Community

Logged-in users can engage with the community:

- **Social feed** — See trips shared by other users, filtered by global, destination-specific, friends, following, or trending
- **Upvote** trips and quest completions
- **Save** trips you like for later
- **Comment** on trips
- **Follow** other travelers (separate from friends — following is one-way, friends is mutual)
- **Report** inappropriate content

---

## 15. Public Library

Anyone (including guests) can browse the public itinerary library:

- **Browse** — See all public itineraries
- **Search** — Natural language search ("3 days in Tokyo on a budget with kids") powered by AI semantic search
- **Trending** — See what's popular right now
- **Destinations** — Browse by destination
- **Clone** — Fork any public itinerary as a starting point for your own trip, then customize it

---

## 16. Export & Share

Logged-in users can export their itinerary in multiple formats:

- **PDF** — Beautiful printable itinerary
- **iCal** — Import into Google Calendar, Apple Calendar, etc.
- **GPX** — Import routes into maps apps
- **Share link** — Shareable URL with preview image and metadata

---

## 17. User Profile

Every account has a profile:

- **Public profile** — Username, avatar, level, trip count, countries visited
- **My trips** — All created trips
- **Saved trips** — Trips saved from the library or feed
- **Stats** — Countries, trips, quests completed, XP breakdown
- **Notifications** — Level-ups, quest completions, social interactions, system messages
- **Settings** — Update profile, avatar, preferences

---

## 18. Admin & Moderation

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
                              │                                   View & Edit (personalized)
                              │                                          │
                              │                         ┌───────────────┼───────────────┐
                              │                         │               │               │
                              │                      Booking     Export/Share   Live Companion
                              │                                                      │
                              │                                          ┌───────────┤
                              │                                          │           │
                              │                                    Check-ins   Group Challenges
                              │                                          │
                              │                                  Rate Activities + Upload Photos
                              │                                          │
                              │                                    Trip Completes
                              │                                          │
                              │                              ┌───────────┤
                              │                              │           │
                              │                       Travel DNA    Trip Memories
                              │                       Updated     (Summary + Highlights + Stats)
                              │
                              ├── My Trips ──→ Active / Upcoming / Completed (Memories)
                              ├── Invite Friends ──→ Shared trip ──→ Group Challenges
                              ├── Library ──→ Browse / Search / Clone
                              ├── Friends ──→ Requests / Mutual / Invite to trips
                              ├── Social Feed ──→ Upvote / Comment / Follow
                              ├── Drift Lore ──→ Solo Quests + Group Side-Quests ──→ XP / Rewards
                              └── Profile ──→ Stats / Achievements / Leaderboards
```

---

## Key Design Principles

1. **No login wall for the core experience** — Anyone can plan a trip and see an AI itinerary without an account
2. **Deeply personalized, never generic** — Every itinerary is dynamically generated based on the user's travel history, preferences, and feedback. Two users going to the same place get different trips
3. **AI does the research** — Users don't need to spend hours on travel blogs; the AI aggregates local knowledge, Reddit tips, weather, costs, and ratings
4. **Every suggestion is local-first** — Tourist traps are flagged, local alternatives are suggested, hidden gems are highlighted
5. **The app gets smarter over time** — Your Travel DNA evolves after every trip, making each itinerary more tuned to what you actually enjoy
6. **Travel is a game** — XP, levels, quests, and leaderboards make exploring feel rewarding
7. **Trips become memories** — Completed trips aren't forgotten — they become a rich, nostalgic timeline with photos, stats, and highlights
8. **Community-powered** — Public trips, social feed, and semantic search mean everyone benefits from everyone else's travels
9. **Real-time companion** — The app doesn't stop being useful once the trip starts; it guides you through each day

---

## Web vs Mobile — Platform Breakdown

Drift is a **web + mobile application**. The backend API serves both platforms identically, but the user experience differs based on what each platform does best.

### Available on Both (Web + Mobile)

| Feature                                    | Notes                                   |
| ------------------------------------------ | --------------------------------------- |
| Guest mode / Sign up / Login               | Same flow on both                       |
| Onboarding wizard                          | Full wizard on both                     |
| AI itinerary generation                    | SSE streaming works on both             |
| View itinerary                             | Day-by-day view on both                 |
| Edit itinerary (swap, reorder, add/remove) | Full editing on both                    |
| Booking                                    | Cart + affiliate checkout on both       |
| Public library (browse, search, clone)     | Full library access                     |
| Social feed                                | Browse, upvote, comment, save           |
| Friends                                    | Send/accept requests, view friends      |
| Follow system                              | Follow/unfollow users                   |
| User profile                               | View/edit profile, stats, achievements  |
| Notifications                              | In-app notifications on both            |
| Drift Lore quests (browse, accept)         | Quest discovery on both                 |
| Leaderboards                               | Global + destination leaderboards       |
| Export (PDF, iCal, share link)             | Available on both                       |
| Admin panel                                | Web-only is fine, but API supports both |

### Mobile-First / Mobile-Only

| Feature                               | Why mobile                                          |
| ------------------------------------- | --------------------------------------------------- |
| **Live trip companion**               | Needs GPS, runs while traveling — inherently mobile |
| **Auto check-in (geofencing)**        | Requires background location access                 |
| **Quest photo upload + verification** | Camera access, EXIF GPS data from phone photos      |
| **Nearby suggestions**                | Real-time location-based recommendations            |
| **Group sync (real-time)**            | Friends traveling together, live location sharing   |
| **Push notifications**                | Trip reminders, quest nudges, friend activity       |
| **GPX export**                        | Import into mobile maps apps (Google Maps, Maps.me) |
| **Offline itinerary access**          | Cache itinerary locally for areas with no signal    |

### Web-First / Web Better

| Feature                       | Why web                                                                |
| ----------------------------- | ---------------------------------------------------------------------- |
| **Onboarding wizard**         | Easier to fill out long forms on desktop                               |
| **Itinerary editing**         | Drag-and-drop, multi-day view is better on large screens               |
| **Library browsing & search** | More comfortable with a full browser                                   |
| **Admin & moderation panel**  | Dashboard-style UI, bulk actions                                       |
| **PDF export preview**        | Easier to preview and print                                            |
| **Trip planning (pre-trip)**  | Most planning happens at a desk before the trip                        |
| **Social feed browsing**      | Scrolling feeds is comfortable on both, but discovery is often desktop |

### Platform Summary

```
PLANNING PHASE (mostly web)
  Onboarding → Generate → Edit → Book → Export
  ├── Desktop: full-featured, comfortable for long planning sessions
  └── Mobile: works fully, just smaller screen

TRAVEL PHASE (mostly mobile)
  Live Companion → Check-ins → Quests → Photo Proof → Nearby
  ├── Mobile: GPS, camera, push notifications — essential
  └── Web: can view itinerary but no live features

SOCIAL PHASE (both equally)
  Feed → Friends → Follow → Comment → Leaderboards
  ├── Web: great for browsing and discovery
  └── Mobile: great for quick interactions and notifications
```
