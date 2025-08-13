# NEARLY Database Schema

## Tables

### users
- id (Primary Key, UUID)
- email (Unique, String)
- password_hash (String)
- created_at (Timestamp)
- updated_at (Timestamp)
- is_active (Boolean, default: True)
- subscription_type (String, default: 'free') # 'free', 'premium'

### profiles
- id (Primary Key, UUID)
- user_id (Foreign Key -> users.id)
- name (String)
- bio (Text)
- profile_picture_url (String)
- age (Integer)
- interests (JSON Array)
- current_latitude (Float)
- current_longitude (Float)
- location_updated_at (Timestamp)
- is_visible (Boolean, default: True)
- created_at (Timestamp)
- updated_at (Timestamp)

### conversations
- id (Primary Key, UUID)
- user1_id (Foreign Key -> users.id)
- user2_id (Foreign Key -> users.id)
- created_at (Timestamp)
- updated_at (Timestamp)
- status (String, default: 'active') # 'active', 'blocked', 'archived'

### messages
- id (Primary Key, UUID)
- conversation_id (Foreign Key -> conversations.id)
- sender_id (Foreign Key -> users.id)
- content (Text)
- sent_at (Timestamp)
- is_read (Boolean, default: False)

### meetups
- id (Primary Key, UUID)
- conversation_id (Foreign Key -> conversations.id)
- proposed_by (Foreign Key -> users.id)
- location_name (String)
- location_latitude (Float)
- location_longitude (Float)
- proposed_time (Timestamp)
- status (String, default: 'proposed') # 'proposed', 'accepted', 'declined', 'completed'
- created_at (Timestamp)
- updated_at (Timestamp)

### feedback
- id (Primary Key, UUID)
- meetup_id (Foreign Key -> meetups.id)
- reviewer_id (Foreign Key -> users.id)
- reviewed_user_id (Foreign Key -> users.id)
- rating (Integer) # 1-5 stars
- comment (Text)
- created_at (Timestamp)

### profile_views
- id (Primary Key, UUID)
- viewer_id (Foreign Key -> users.id)
- viewed_profile_id (Foreign Key -> profiles.id)
- viewed_at (Timestamp)

## Indexes
- profiles.current_latitude, profiles.current_longitude (for location queries)
- messages.conversation_id, messages.sent_at (for message ordering)
- conversations.user1_id, conversations.user2_id (for user conversations)
- profile_views.viewer_id, profile_views.viewed_at (for view history)

