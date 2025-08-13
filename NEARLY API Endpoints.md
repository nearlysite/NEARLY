# NEARLY API Endpoints

## Authentication Endpoints

### POST /api/auth/register
Register a new user
- Request: `{ "email": "string", "password": "string" }`
- Response: `{ "user_id": "uuid", "token": "jwt_token" }`

### POST /api/auth/login
Login user
- Request: `{ "email": "string", "password": "string" }`
- Response: `{ "user_id": "uuid", "token": "jwt_token" }`

### POST /api/auth/logout
Logout user (invalidate token)
- Headers: `Authorization: Bearer <token>`
- Response: `{ "message": "Logged out successfully" }`

## Profile Endpoints

### GET /api/profile
Get current user's profile
- Headers: `Authorization: Bearer <token>`
- Response: `{ "id": "uuid", "name": "string", "bio": "string", "profile_picture_url": "string", "age": "int", "interests": ["string"] }`

### PUT /api/profile
Update current user's profile
- Headers: `Authorization: Bearer <token>`
- Request: `{ "name": "string", "bio": "string", "age": "int", "interests": ["string"] }`
- Response: `{ "message": "Profile updated successfully" }`

### POST /api/profile/picture
Upload profile picture
- Headers: `Authorization: Bearer <token>`
- Request: Form data with image file
- Response: `{ "profile_picture_url": "string" }`

### PUT /api/profile/location
Update current location
- Headers: `Authorization: Bearer <token>`
- Request: `{ "latitude": "float", "longitude": "float" }`
- Response: `{ "message": "Location updated successfully" }`

## Discovery Endpoints

### GET /api/nearby
Get nearby users
- Headers: `Authorization: Bearer <token>`
- Query params: `?radius=1000&limit=20`
- Response: `{ "users": [{ "id": "uuid", "name": "string", "bio": "string", "profile_picture_url": "string", "age": "int", "distance": "float", "interests": ["string"] }] }`

### GET /api/profile/{user_id}
Get another user's profile
- Headers: `Authorization: Bearer <token>`
- Response: `{ "id": "uuid", "name": "string", "bio": "string", "profile_picture_url": "string", "age": "int", "interests": ["string"] }`

## Messaging Endpoints

### GET /api/conversations
Get user's conversations
- Headers: `Authorization: Bearer <token>`
- Response: `{ "conversations": [{ "id": "uuid", "other_user": { "id": "uuid", "name": "string", "profile_picture_url": "string" }, "last_message": "string", "last_message_time": "timestamp", "unread_count": "int" }] }`

### POST /api/conversations
Start a new conversation
- Headers: `Authorization: Bearer <token>`
- Request: `{ "user_id": "uuid" }`
- Response: `{ "conversation_id": "uuid" }`

### GET /api/conversations/{conversation_id}/messages
Get messages in a conversation
- Headers: `Authorization: Bearer <token>`
- Query params: `?limit=50&offset=0`
- Response: `{ "messages": [{ "id": "uuid", "sender_id": "uuid", "content": "string", "sent_at": "timestamp", "is_read": "boolean" }], "message_count": "int", "can_send_more": "boolean" }`

### POST /api/conversations/{conversation_id}/messages
Send a message
- Headers: `Authorization: Bearer <token>`
- Request: `{ "content": "string" }`
- Response: `{ "message_id": "uuid", "sent_at": "timestamp", "remaining_free_messages": "int" }`

### PUT /api/conversations/{conversation_id}/read
Mark messages as read
- Headers: `Authorization: Bearer <token>`
- Response: `{ "message": "Messages marked as read" }`

## Meetup Endpoints

### POST /api/meetups
Propose a meetup
- Headers: `Authorization: Bearer <token>`
- Request: `{ "conversation_id": "uuid", "location_name": "string", "location_latitude": "float", "location_longitude": "float", "proposed_time": "timestamp" }`
- Response: `{ "meetup_id": "uuid" }`

### PUT /api/meetups/{meetup_id}/respond
Respond to a meetup proposal
- Headers: `Authorization: Bearer <token>`
- Request: `{ "status": "accepted|declined" }`
- Response: `{ "message": "Meetup response recorded" }`

### GET /api/meetups
Get user's meetups
- Headers: `Authorization: Bearer <token>`
- Response: `{ "meetups": [{ "id": "uuid", "conversation_id": "uuid", "other_user": { "name": "string", "profile_picture_url": "string" }, "location_name": "string", "proposed_time": "timestamp", "status": "string" }] }`

## Feedback Endpoints

### POST /api/feedback
Leave feedback after a meetup
- Headers: `Authorization: Bearer <token>`
- Request: `{ "meetup_id": "uuid", "reviewed_user_id": "uuid", "rating": "int", "comment": "string" }`
- Response: `{ "message": "Feedback submitted successfully" }`

## Premium Features Endpoints

### GET /api/profile/views
Get who viewed your profile (premium only)
- Headers: `Authorization: Bearer <token>`
- Response: `{ "views": [{ "viewer": { "id": "uuid", "name": "string", "profile_picture_url": "string" }, "viewed_at": "timestamp" }] }`

### POST /api/profile/boost
Boost profile visibility (premium only)
- Headers: `Authorization: Bearer <token>`
- Response: `{ "message": "Profile boosted successfully", "boost_expires_at": "timestamp" }`

## Subscription Endpoints

### GET /api/subscription
Get current subscription status
- Headers: `Authorization: Bearer <token>`
- Response: `{ "type": "free|premium", "expires_at": "timestamp|null" }`

### POST /api/subscription/upgrade
Upgrade to premium
- Headers: `Authorization: Bearer <token>`
- Request: `{ "payment_token": "string" }`
- Response: `{ "message": "Subscription upgraded successfully" }`

## WebSocket Events (Real-time messaging)

### Connection
- URL: `/ws/chat?token=<jwt_token>`

### Events
- `message_received`: New message in conversation
- `user_online`: User came online
- `user_offline`: User went offline
- `typing_start`: User started typing
- `typing_stop`: User stopped typing

