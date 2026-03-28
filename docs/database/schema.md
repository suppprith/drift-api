# Database Schema

## Core Tables

### profiles

Extends Supabase `auth.users`.

```sql
CREATE TABLE profiles (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  username TEXT UNIQUE,
  display_name TEXT,
  avatar_url TEXT,
  home_city TEXT,
  home_country TEXT,
  home_lat FLOAT,
  home_lng FLOAT,
  travel_style_summary TEXT,
  xp INTEGER DEFAULT 0,
  level INTEGER DEFAULT 1,
  level_name TEXT DEFAULT 'Wanderer',
  total_trips INTEGER DEFAULT 0,
  total_countries INTEGER DEFAULT 0,
  total_lore_completed INTEGER DEFAULT 0,
  preferences JSONB DEFAULT '{}',
  travel_dna JSONB DEFAULT '{}', -- evolving AI-learned preferences (top categories, pace, adventure level, etc.)
  is_pro BOOLEAN DEFAULT FALSE,
  pro_expires_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### trips

The core entity.

```sql
CREATE TABLE trips (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES profiles(id) ON DELETE SET NULL,
  slug TEXT UNIQUE NOT NULL,
  title TEXT NOT NULL,
  status TEXT DEFAULT 'draft' CHECK (status IN ('draft','generated','upcoming','active','completed','archived')),
  is_public BOOLEAN DEFAULT TRUE,

  -- Origin
  origin_city TEXT NOT NULL,
  origin_country TEXT NOT NULL,
  origin_lat FLOAT NOT NULL,
  origin_lng FLOAT NOT NULL,

  -- Destinations (supports multi-city V2)
  destinations JSONB NOT NULL, -- [{city, country, lat, lng, days_allocated}]

  -- Guest access
  guest_session_id TEXT, -- non-null for guest-created trips, cleared on account migration

  -- Dates
  start_date DATE NOT NULL,
  end_date DATE NOT NULL,
  duration_nights INTEGER NOT NULL,
  duration_days INTEGER NOT NULL,

  -- Preferences
  transport_mode TEXT,
  transport_details JSONB,
  group_type TEXT CHECK (group_type IN ('solo','couple','friends','family')),
  group_size INTEGER DEFAULT 1,
  has_kids BOOLEAN DEFAULT FALSE,
  budget_tier TEXT CHECK (budget_tier IN ('budget','mid-range','luxury')),
  travel_pace TEXT CHECK (travel_pace IN ('slow','balanced','packed')),
  getting_around TEXT CHECK (getting_around IN ('walk','mix','taxi')),
  food_preferences TEXT[] DEFAULT '{}',
  food_allergies TEXT,
  accommodation_type TEXT,
  vibes TEXT[] DEFAULT '{}',
  accessibility_needed BOOLEAN DEFAULT FALSE,
  accessibility_notes TEXT,
  specific_requests TEXT,
  selected_interests TEXT[] DEFAULT '{}',

  -- Generation metadata
  ai_model TEXT,
  generation_time_ms INTEGER,
  prompt_tokens INTEGER,
  completion_tokens INTEGER,

  -- Aggregates
  estimated_total_cost_local FLOAT,
  estimated_total_cost_usd FLOAT,
  local_currency TEXT,
  upvote_count INTEGER DEFAULT 0,
  save_count INTEGER DEFAULT 0,
  clone_count INTEGER DEFAULT 0,
  view_count INTEGER DEFAULT 0,

  -- Must-do tracking
  must_do_status JSONB DEFAULT '[]',

  -- Memory (generated after trip completion)
  memory_summary TEXT, -- AI-generated nostalgia summary of the trip
  memory_highlights JSONB DEFAULT '[]', -- [{type, title, description, photo_url, activity_id}]
  memory_stats JSONB, -- {activities_completed, activities_skipped, quests_done, xp_earned, km_walked, ...}
  completed_at TIMESTAMPTZ, -- when the trip actually ended

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### trip_members

Tracks who is part of a trip (collaborators, not just the owner).

```sql
CREATE TABLE trip_members (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  trip_id UUID NOT NULL REFERENCES trips(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  role TEXT NOT NULL DEFAULT 'member' CHECK (role IN ('owner','editor','viewer')),
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending','accepted','declined')),
  invited_by UUID REFERENCES profiles(id) ON DELETE SET NULL,
  joined_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(trip_id, user_id)
);
```

### itinerary_days

```sql
CREATE TABLE itinerary_days (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  trip_id UUID REFERENCES trips(id) ON DELETE CASCADE,
  day_number INTEGER NOT NULL,
  date DATE NOT NULL,
  destination_city TEXT NOT NULL,
  destination_country TEXT NOT NULL,

  -- Weather
  weather_summary TEXT,
  weather_icon TEXT,
  temp_high_c FLOAT,
  temp_low_c FLOAT,
  rain_probability FLOAT,

  -- Daily spend
  estimated_spend_local FLOAT,
  estimated_spend_usd FLOAT,
  running_total_local FLOAT,
  running_total_usd FLOAT,

  -- Metadata
  day_summary TEXT,
  is_transit_day BOOLEAN DEFAULT FALSE,
  sort_order INTEGER NOT NULL,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(trip_id, day_number)
);
```

### itinerary_activities

```sql
CREATE TABLE itinerary_activities (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  day_id UUID REFERENCES itinerary_days(id) ON DELETE CASCADE,
  trip_id UUID REFERENCES trips(id) ON DELETE CASCADE,

  -- Timing
  start_time TIME NOT NULL,
  end_time TIME,
  duration_minutes INTEGER,

  -- Activity details
  category TEXT NOT NULL CHECK (category IN ('food','attraction','experience','nature','transport','accommodation','shopping','nightlife','wellness','culture')),
  name TEXT NOT NULL,
  description TEXT,
  address TEXT,
  neighborhood TEXT,
  lat FLOAT,
  lng FLOAT,

  -- Pricing
  cost_local FLOAT,
  cost_usd FLOAT,
  cost_currency TEXT,
  payment_methods TEXT[],

  -- Local intelligence
  local_tip TEXT,
  crowd_timing TEXT,
  booking_urgency TEXT,
  is_tourist_trap BOOLEAN DEFAULT FALSE,
  local_alternative TEXT,
  is_local_pick BOOLEAN DEFAULT FALSE,

  -- Food specific
  order_recommendation TEXT,

  -- Rain backup
  rain_backup_name TEXT,
  rain_backup_description TEXT,
  rain_backup_lat FLOAT,
  rain_backup_lng FLOAT,

  -- Social proof
  tripadvisor_rating FLOAT,
  tripadvisor_review_count INTEGER,
  tripadvisor_url TEXT,

  -- Booking
  is_bookable BOOLEAN DEFAULT FALSE,
  booking_url TEXT,
  booking_provider TEXT,
  booking_price_usd FLOAT,

  -- Flags
  is_must_do BOOLEAN DEFAULT FALSE,
  is_hidden_gem BOOLEAN DEFAULT FALSE,
  is_ai_generated BOOLEAN DEFAULT TRUE,
  is_user_added BOOLEAN DEFAULT FALSE,

  sort_order INTEGER NOT NULL,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### transit_segments

```sql
CREATE TABLE transit_segments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  trip_id UUID REFERENCES trips(id) ON DELETE CASCADE,
  day_id UUID REFERENCES itinerary_days(id) ON DELETE CASCADE,
  from_activity_id UUID REFERENCES itinerary_activities(id) ON DELETE CASCADE,
  to_activity_id UUID REFERENCES itinerary_activities(id) ON DELETE CASCADE,

  mode TEXT NOT NULL CHECK (mode IN ('walk','taxi','bus','train','metro','ferry','bike','scooter','car','rickshaw')),
  duration_minutes INTEGER,
  distance_km FLOAT,
  cost_local FLOAT,
  cost_usd FLOAT,
  route_polyline TEXT,
  notes TEXT,

  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## Cache Tables

### destination_cache

Shared across all users.

```sql
CREATE TABLE destination_cache (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  destination_key TEXT UNIQUE NOT NULL, -- "pokhara-nepal"
  city TEXT NOT NULL,
  country TEXT NOT NULL,
  lat FLOAT,
  lng FLOAT,

  destination_research JSONB,
  research_cached_at TIMESTAMPTZ,

  must_dos JSONB DEFAULT '[]',
  must_dos_cached_at TIMESTAMPTZ,

  reddit_intel JSONB,
  reddit_cached_at TIMESTAMPTZ,

  interest_tiles JSONB,
  interest_tiles_cached_at TIMESTAMPTZ,

  transport_options JSONB DEFAULT '{}',

  timezone TEXT,
  default_currency TEXT,
  language TEXT,
  visa_info JSONB,
  safety_rating TEXT,
  best_months TEXT[],

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### weather_cache

```sql
CREATE TABLE weather_cache (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  location_key TEXT NOT NULL, -- "lat:lng"
  date DATE NOT NULL,
  source TEXT NOT NULL CHECK (source IN ('forecast','historical','climate')),

  weather_data JSONB NOT NULL,
  cached_at TIMESTAMPTZ DEFAULT NOW(),
  expires_at TIMESTAMPTZ NOT NULL,

  UNIQUE(location_key, date, source)
);
```

---

## Gamification Tables

### lore_quests

```sql
CREATE TABLE lore_quests (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  destination_key TEXT NOT NULL,

  quest_type TEXT NOT NULL CHECK (quest_type IN ('destination_lore','touch_grass','dad_lore','mythic')),
  title TEXT NOT NULL,
  description TEXT NOT NULL,
  emoji TEXT,

  geofence_lat FLOAT,
  geofence_lng FLOAT,
  geofence_radius_m INTEGER DEFAULT 500,
  required_photo_subject TEXT,
  verification_prompt TEXT,

  xp_reward INTEGER NOT NULL DEFAULT 50,
  bonus_reward JSONB,

  difficulty TEXT CHECK (difficulty IN ('easy','medium','hard','mythic')),
  estimated_time_minutes INTEGER,
  is_seasonal BOOLEAN DEFAULT FALSE,
  available_months INTEGER[],
  min_level INTEGER DEFAULT 1,

  completion_count INTEGER DEFAULT 0,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### quest_completions

```sql
CREATE TABLE quest_completions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,
  quest_id UUID REFERENCES lore_quests(id) ON DELETE CASCADE,
  trip_id UUID REFERENCES trips(id) ON DELETE SET NULL,

  photo_url TEXT NOT NULL,
  photo_r2_key TEXT NOT NULL,
  photo_lat FLOAT,
  photo_lng FLOAT,
  photo_taken_at TIMESTAMPTZ,

  ai_verification_status TEXT DEFAULT 'pending' CHECK (ai_verification_status IN ('pending','verified','rejected','manual_review')),
  ai_verification_result JSONB,
  ai_confidence_score FLOAT,

  moderation_status TEXT DEFAULT 'pending' CHECK (moderation_status IN ('pending','approved','flagged','removed')),
  moderation_flags TEXT[],

  xp_granted INTEGER DEFAULT 0,
  bonus_granted JSONB,

  caption TEXT,
  like_count INTEGER DEFAULT 0,
  comment_count INTEGER DEFAULT 0,
  is_featured BOOLEAN DEFAULT FALSE,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(user_id, quest_id)
);
```

### location_checkins

```sql
CREATE TABLE location_checkins (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,
  trip_id UUID REFERENCES trips(id) ON DELETE CASCADE,
  activity_id UUID REFERENCES itinerary_activities(id) ON DELETE SET NULL,

  lat FLOAT NOT NULL,
  lng FLOAT NOT NULL,
  checked_in_at TIMESTAMPTZ DEFAULT NOW(),
  auto_detected BOOLEAN DEFAULT FALSE
);
```

---

## Booking Tables

### booking_items

```sql
CREATE TABLE booking_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,
  trip_id UUID REFERENCES trips(id) ON DELETE CASCADE,
  activity_id UUID REFERENCES itinerary_activities(id) ON DELETE SET NULL,

  item_type TEXT NOT NULL CHECK (item_type IN ('flight','hotel','experience','restaurant','transport','insurance')),
  provider TEXT,
  provider_item_id TEXT,

  title TEXT NOT NULL,
  description TEXT,
  date DATE,

  quantity INTEGER DEFAULT 1,
  unit_price_local FLOAT,
  unit_price_usd FLOAT,
  currency TEXT,

  booking_url TEXT,
  affiliate_tag TEXT,

  status TEXT DEFAULT 'cart' CHECK (status IN ('cart','booked','cancelled','refunded')),
  booking_reference TEXT,
  booking_confirmed_at TIMESTAMPTZ,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## Social Tables

### trip_upvotes

```sql
CREATE TABLE trip_upvotes (
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,
  trip_id UUID REFERENCES trips(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  PRIMARY KEY (user_id, trip_id)
);
```

### friendships

```sql
CREATE TABLE friendships (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  requester_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  addressee_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  status TEXT NOT NULL DEFAULT 'pending' CHECK (status IN ('pending','accepted','declined','blocked')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(requester_id, addressee_id)
);
```

### trip_saves

```sql
CREATE TABLE trip_saves (
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,
  trip_id UUID REFERENCES trips(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  PRIMARY KEY (user_id, trip_id)
);
```

### comments

```sql
CREATE TABLE comments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,
  target_type TEXT NOT NULL CHECK (target_type IN ('trip','quest_completion','comment')),
  target_id UUID NOT NULL,
  parent_id UUID REFERENCES comments(id) ON DELETE CASCADE,

  body TEXT NOT NULL,
  like_count INTEGER DEFAULT 0,
  is_edited BOOLEAN DEFAULT FALSE,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### notifications

```sql
CREATE TABLE notifications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,

  type TEXT NOT NULL,
  title TEXT NOT NULL,
  body TEXT,
  data JSONB DEFAULT '{}',

  is_read BOOLEAN DEFAULT FALSE,
  read_at TIMESTAMPTZ,

  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### user_achievements

```sql
CREATE TABLE user_achievements (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,

  achievement_key TEXT NOT NULL,
  achievement_name TEXT NOT NULL,
  achievement_description TEXT,
  achievement_icon TEXT,

  unlocked_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(user_id, achievement_key)
);
```

### reports

```sql
CREATE TABLE reports (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  reporter_id UUID REFERENCES profiles(id) ON DELETE CASCADE,

  target_type TEXT NOT NULL CHECK (target_type IN ('trip','quest_completion','comment','profile')),
  target_id UUID NOT NULL,

  reason TEXT NOT NULL CHECK (reason IN ('spam','inappropriate','inaccurate','harassment','other')),
  description TEXT,

  status TEXT DEFAULT 'open' CHECK (status IN ('open','reviewing','resolved','dismissed')),
  resolved_by UUID REFERENCES profiles(id),
  resolved_at TIMESTAMPTZ,

  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## Group Challenges Tables

### group_challenges

Challenges that trip members can do together during a shared trip.

```sql
CREATE TABLE group_challenges (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  trip_id UUID NOT NULL REFERENCES trips(id) ON DELETE CASCADE,
  created_by UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,

  challenge_type TEXT NOT NULL CHECK (challenge_type IN ('race','collective','versus','scavenger_hunt','streak')),
  title TEXT NOT NULL,
  description TEXT,
  emoji TEXT,

  -- Conditions
  target_type TEXT CHECK (target_type IN ('quest','checkin','photo','activity','custom')),
  target_count INTEGER DEFAULT 1, -- e.g. "check in at 5 places"
  target_quest_ids UUID[], -- specific quests if quest-based
  deadline TIMESTAMPTZ, -- optional time limit

  -- Rewards
  xp_reward INTEGER DEFAULT 100,
  bonus_reward JSONB, -- nullable bonus for winner/completers

  status TEXT DEFAULT 'active' CHECK (status IN ('active','completed','expired','cancelled')),
  winner_id UUID REFERENCES profiles(id) ON DELETE SET NULL,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### group_challenge_progress

Tracks each member's progress on a group challenge.

```sql
CREATE TABLE group_challenge_progress (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  challenge_id UUID NOT NULL REFERENCES group_challenges(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,

  progress INTEGER DEFAULT 0,
  completed BOOLEAN DEFAULT FALSE,
  completed_at TIMESTAMPTZ,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(challenge_id, user_id)
);
```

---

## Personalization & Memories Tables

### activity_feedback

User feedback on activities — drives the personalization engine and travel DNA evolution.

```sql
CREATE TABLE activity_feedback (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  trip_id UUID NOT NULL REFERENCES trips(id) ON DELETE CASCADE,
  activity_id UUID NOT NULL REFERENCES itinerary_activities(id) ON DELETE CASCADE,

  rating TEXT NOT NULL CHECK (rating IN ('loved','liked','neutral','disliked')),
  comment TEXT, -- optional short note ("food was incredible", "too crowded")
  was_skipped BOOLEAN DEFAULT FALSE, -- from live companion skip data
  actual_duration_minutes INTEGER, -- how long they actually stayed (from check-in data)

  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(user_id, activity_id)
);
```

### trip_photos

General trip photos (not quest-related). Users can upload memories from their trip.

```sql
CREATE TABLE trip_photos (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  trip_id UUID NOT NULL REFERENCES trips(id) ON DELETE CASCADE,
  activity_id UUID REFERENCES itinerary_activities(id) ON DELETE SET NULL, -- optional link to an activity
  day_id UUID REFERENCES itinerary_days(id) ON DELETE SET NULL, -- optional link to a day

  photo_url TEXT NOT NULL,
  photo_r2_key TEXT NOT NULL,
  caption TEXT,
  lat FLOAT,
  lng FLOAT,
  taken_at TIMESTAMPTZ,

  moderation_status TEXT DEFAULT 'approved' CHECK (moderation_status IN ('approved','flagged','removed')),

  created_at TIMESTAMPTZ DEFAULT NOW()
);
```
