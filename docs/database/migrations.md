# Database Migrations & Indexes

## Indexes

```sql
-- Trips
CREATE INDEX idx_trips_user_id ON trips(user_id);
CREATE INDEX idx_trips_slug ON trips(slug);
CREATE INDEX idx_trips_status ON trips(status);
CREATE INDEX idx_trips_public ON trips(is_public) WHERE is_public = TRUE;
CREATE INDEX idx_trips_destination ON trips USING GIN(destinations);

-- Trip Members
CREATE INDEX idx_trip_members_trip ON trip_members(trip_id, status);
CREATE INDEX idx_trip_members_user ON trip_members(user_id, status);

-- Itinerary
CREATE INDEX idx_itinerary_days_trip ON itinerary_days(trip_id);
CREATE INDEX idx_activities_day ON itinerary_activities(day_id);
CREATE INDEX idx_activities_trip ON itinerary_activities(trip_id);
CREATE INDEX idx_activities_category ON itinerary_activities(category);

-- Cache
CREATE INDEX idx_destination_cache_key ON destination_cache(destination_key);
CREATE INDEX idx_weather_cache_lookup ON weather_cache(location_key, date);

-- Lore
CREATE INDEX idx_lore_quests_dest ON lore_quests(destination_key);
CREATE INDEX idx_quest_completions_user ON quest_completions(user_id);
CREATE INDEX idx_quest_completions_quest ON quest_completions(quest_id);

-- Live
CREATE INDEX idx_checkins_user_trip ON location_checkins(user_id, trip_id);

-- Booking
CREATE INDEX idx_booking_items_user ON booking_items(user_id, trip_id);

-- Social
CREATE INDEX idx_comments_target ON comments(target_type, target_id);
CREATE INDEX idx_notifications_user ON notifications(user_id, is_read);

-- Friends
CREATE INDEX idx_friendships_requester ON friendships(requester_id, status);
CREATE INDEX idx_friendships_addressee ON friendships(addressee_id, status);
CREATE UNIQUE INDEX idx_friendships_pair ON friendships(LEAST(requester_id, addressee_id), GREATEST(requester_id, addressee_id));

-- Admin
CREATE INDEX idx_reports_status ON reports(status);

-- Group Challenges
CREATE INDEX idx_group_challenges_trip ON group_challenges(trip_id, status);
CREATE INDEX idx_group_challenge_progress_challenge ON group_challenge_progress(challenge_id);
CREATE INDEX idx_group_challenge_progress_user ON group_challenge_progress(user_id);

-- Activity Feedback
CREATE INDEX idx_activity_feedback_user ON activity_feedback(user_id);
CREATE INDEX idx_activity_feedback_trip ON activity_feedback(trip_id);
CREATE INDEX idx_activity_feedback_activity ON activity_feedback(activity_id);

-- Trip Photos
CREATE INDEX idx_trip_photos_user ON trip_photos(user_id);
CREATE INDEX idx_trip_photos_trip ON trip_photos(trip_id);

-- Trip History / Memories
CREATE INDEX idx_trips_completed ON trips(user_id, status) WHERE status = 'completed';
```

## Migration Order

1. `001_create_profiles.sql` — profiles table (includes travel_dna JSONB)
2. `002_create_trips.sql` — trips table (includes memory_summary, memory_highlights, memory_stats, completed_at)
3. `003_create_itinerary_days.sql` — itinerary_days table
4. `004_create_itinerary_activities.sql` — itinerary_activities table
5. `005_create_transit_segments.sql` — transit_segments table
6. `006_create_destination_cache.sql` — destination_cache table
7. `007_create_weather_cache.sql` — weather_cache table
8. `008_create_lore_quests.sql` — lore_quests table
9. `009_create_quest_completions.sql` — quest_completions table
10. `010_create_location_checkins.sql` — location_checkins table
11. `011_create_booking_items.sql` — booking_items table
12. `012_create_trip_upvotes.sql` — trip_upvotes table
13. `013_create_trip_saves.sql` — trip_saves table
14. `014_create_comments.sql` — comments table
15. `015_create_notifications.sql` — notifications table
16. `016_create_user_achievements.sql` — user_achievements table
17. `017_create_reports.sql` — reports table
18. `018_create_trip_members.sql` — trip_members table
19. `019_create_group_challenges.sql` — group_challenges table
20. `020_create_group_challenge_progress.sql` — group_challenge_progress table
21. `021_create_activity_feedback.sql` — activity_feedback table
22. `022_create_trip_photos.sql` — trip_photos table
23. `023_create_indexes.sql` — all indexes above
