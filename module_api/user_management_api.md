# User Management API API Documentation

Group: Cocotree

# User Management API Documentation

## URL `/users/user_info/<int:id>/`

Description: Retrieves basic user information by user ID.

### GET

Description: Returns basic user info including ID, username, profile picture, bio, and friendship status.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "id": 123,
    "username": "example_user",
    "bio": "User's bio text",
    "profile_picture": "http://example.com/media/profile_pics/user123.jpg",
    "created_time": "2023-07-01T12:00:00Z",
    "is_friend": true,
    "group": "Close Friends"
  }
}
```

#### Error Responses

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User not found"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 1,
  "error": "Authentication credentials were not provided."
}
```

## URL `/users/search_users/`

Description: Searches for users by exact username match.

### GET

Description: Returns user information for the exact username specified.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Query Parameters
- username: The exact username to search for

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "id": 123,
    "username": "example_user",
    "profile_picture": "http://example.com/media/profile_pics/user123.jpg"
  }
}
```

#### Error Responses

**HTTP 404 Not Found**
```json
{
  "code": 1,
  "error": "User not found"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Username is required"
}
```

## URL `/users/search_friends/`

Description: Searches for friends of the current user with optional filtering.

### GET

Description: Returns a list of the user's friends, optionally filtered by username prefix and/or group.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Query Parameters
- prefix (optional): Only include friends whose username starts with this (case-insensitive)
- group (optional): Only include friends in this group

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "friends": [
      {
        "id": 123,
        "username": "example_friend",
        "profile_picture": "http://example.com/media/profile_pics/friend123.jpg",
        "group": "Close Friends"
      }
    ]
  }
}
```

## URL `/users/user_details/`

Description: Manages user profile information.

### GET

Description: Retrieves the current user's complete profile information.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "id": 123,
    "username": "example_user",
    "email": "user@example.com",
    "phone_number": "+1234567890",
    "bio": "User's bio text",
    "profile_picture": "http://example.com/media/profile_pics/user123.jpg",
    "created_time": "2023-07-01T12:00:00Z",
    "pending_count": 2
  }
}
```

### PATCH

Description: Updates the user's non-sensitive information (username, profile picture, bio).

#### Request Header Format
- Authorization: <JWT_TOKEN>
- Content-Type: application/json

#### Request Body Format
```json
{
  "username": "new_username",
  "bio": "Updated bio information",
  "profile_picture": "[file upload]"
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "User info updated successfully",
    "data": {
      "id": 123,
      "username": "new_username",
      "bio": "Updated bio information",
      "profile_picture": "http://example.com/media/profile_pics/new_pic.jpg"
    },
    "token": "updated.jwt.token"  
  }
}
```

#### Error Responses

**HTTP 400 Bad Request**
```json
{
  "error": "Username already exists",
  "payload": {
    "username": ["A user with that username already exists."]
  },
  "code": 1
}
```

## URL `/users/user_details/sensitive/`

Description: Manages sensitive user information.

### PATCH

Description: Updates sensitive user information (password, email, phone number). Requires current password confirmation.

#### Request Header Format
- Authorization: <JWT_TOKEN>
- Content-Type: application/json

#### Request Body Format
```json
{
  "validation_password": "encrypted_current_password",
  "password": "encrypted_new_password",
  "email": "new_email@example.com",
  "phone_number": "+1987654321"
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "User info updated successfully",
    "data": {
      "id": 123,
      "email": "new_email@example.com",
      "phone_number": "+1987654321"
    }
  }
}
```

#### Error Responses

**HTTP 401 Unauthorized**
```json
{
  "code": 1,
  "error": "Current password is incorrect"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Current password is required"
}
```

## URL `/users/delete_account/`

Description: Allows a user to delete their account.

### DELETE

Description: Deactivates the user account, anonymizes data, and cleans up related information including friendships and chat memberships.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Successful Response
```json
{
  "message": "Account successfully deactivated"
}
```

## URL `/users/friends/request/`

Description: Manages sending friend requests.

### POST

Description: Sends a friend request to another user. If there's already a pending request from the recipient, it will automatically accept it.

#### Request Header Format
- Authorization: <JWT_TOKEN>
- Content-Type: application/json

#### Request Body Format
```json
{
  "id": 456
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "Friend request sent successfully",
    "auto_accepted": false
  }
}
```

#### Error Responses

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Cannot send friend request to yourself"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 3,
  "error": "Already friends with this user"
}
```

## URL `/users/friends/respond/<int:request_id>/`

Description: Manages response to friend requests.

### POST

Description: Accepts or rejects a friend request. When accepting, creates a bidirectional friendship and sets up a private chat.

#### Request Header Format
- Authorization: <JWT_TOKEN>
- Content-Type: application/json

#### Request Body Format
```json
{
  "response": "accept"
}
```
or
```json
{
  "response": "reject"
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "Friend request accepted"
  }
}
```
or
```json
{
  "code": 0,
  "payload": {
    "message": "Friend request rejected"
  }
}
```

#### Error Responses

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Request not found"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Invalid response type"
}
```

## URL `/users/friends/delete/<int:friend_id>/`

Description: Deletes a friendship.

### DELETE

Description: Removes both sides of a friendship and updates the private chat with a system message.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "Friend removed successfully"
  }
}
```

#### Error Responses

**HTTP 404 Not Found**
```json
{
  "code": 1,
  "error": "User not found"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Friendship asymmetric OR not found"
}
```

## URL `/users/friends/groups/`

Description: Manages friend groups.

### GET

Description: Retrieves all friend groups for the current user.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "groups": ["Default", "Close Friends", "Family", "Colleagues"]
  }
}
```

## URL `/users/friends/received_requests/`

Description: Retrieves friend requests received by the user.

### GET

Description: Gets all pending friend requests plus recent non-pending requests.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Query Parameters
- count (optional): Number of recent non-pending requests to include (default=5)

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "pending": [
      {
        "id": 789,
        "from_user": {
          "id": 456,
          "username": "requestor_user",
          "profile_picture": "http://example.com/media/profile_pics/user456.jpg"
        },
        "last_updated": 1628097600
      }
    ],
    "recent": [
      {
        "id": 123,
        "user": {
          "id": 789,
          "username": "other_user",
          "profile_picture": "http://example.com/media/profile_pics/user789.jpg"
        },
        "status": "accepted",
        "last_updated": "2023-08-04T15:30:00Z",
        "direction": "to"
      }
    ]
  }
}
```

## URL `/users/friends/sent_requests/`

Description: Retrieves friend requests sent by the user.

### GET

Description: Gets all pending friend requests sent by the current user.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "pending": [
      {
        "id": 123,
        "to_user": {
          "id": 456,
          "username": "recipient_user",
          "profile_picture": "http://example.com/media/profile_pics/user456.jpg"
        },
        "last_updated": 1628097600
      }
    ]
  }
}
```

## URL `/users/friends/group_update/<int:friend_id>/`

Description: Updates the group assignment for a friend.

### PATCH

Description: Changes the group a friend belongs to.

#### Request Header Format
- Authorization: <JWT_TOKEN>
- Content-Type: application/json

#### Request Body Format
```json
{
  "group": "Close Friends"
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "Friend group updated",
    "group": "Close Friends"
  }
}
```

#### Error Responses

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Friendship not found"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Group name too long"
}
```