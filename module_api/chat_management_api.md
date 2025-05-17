# Chat Management API Documentation

Group: Cocotree

## URL `/chat/chatlist/`

Description: Retrieves user's chat list with details including unread messages, latest message info, and more.

### GET

Description: Returns all chats the authenticated user is part of, ordered by pinned status first, then by latest message timestamp.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "chats": [
      {
        "chat_id": 1,
        "chat_name": "Chat 1",
        "created_at": "2023-10-01T12:00:00Z",
        "unread_messages_count": 5,
        "latest_message_username": "User 1",
        "latest_message_type": "text",
        "latest_message_content": "Hello!",
        "muted": false,
        "pinned": true,
        "private": false,
        "chat_profile_picture": "http://example.com/media/chat_profile/1.jpg"
      }
    ]
  }
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User does not exist"
}
```

## URL `/chat/get_chat_info/<str:chat_id>/`

Description: Retrieves detailed information about a specific chat.

### GET

Description: Returns chat details including members, announcements, and profile picture.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "chat": {
      "chat_name": "Chat Name",
      "private": false,
      "chat_members": [
        {
          "user_id": 1,
          "username": "user1",
          "profile_picture": "http://example.com/media/profile/1.jpg",
          "last_access_time": "2023-10-01T12:00:00Z",
          "role": "owner",
          "is_deleted": false
        }
      ],
      "chat_announcements": [
        {
          "announcement_id": 1,
          "content": "Important announcement",
          "timestamp": "2023-10-01T12:00:00Z"
        }
      ],
      "chat_profile_picture": "http://example.com/media/chat_profile/1.jpg"
    }
  }
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Chat does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User is not a member of this chat"
}
```

## URL `/chat/create_chat/`

Description: Creates a new group chat with specified members.

### POST

Description: Creates a new chat with the authenticated user as owner and adds specified members.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{
  "chat_name": "New Chat Name",
  "chat_members": [1, 2, 3]
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "chat": {
      "id": 1,
      "chat_name": "New Chat Name",
      "private": false,
      "created_time": "2023-10-01T12:00:00Z"
    }
  }
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 2,
  "error": "Group Chat must have at least 3 members"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User does not exist"
}
```

## URL `/chat/delete_chat/<str:chat_id>/`

Description: Deletes a chat and all associated data.

### DELETE

Description: Removes the chat and notifies all members. Only the owner can delete a chat.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User or group chat does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "No permission to delete chat"
}
```

## URL `/chat/toggle_mute/<str:chat_id>/`

Description: Toggles the mute status of a chat for the authenticated user.

### PATCH

Description: Mutes or unmutes the chat for the authenticated user.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "muted": true
  }
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Chat not found"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User is not a member of this chat"
}
```

## URL `/chat/toggle_pinned/<str:chat_id>/`

Description: Toggles the pinned status of a chat for the authenticated user.

### PATCH

Description: Pins or unpins the chat for the authenticated user.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "pinned": true
  }
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Chat not found"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User is not a member of this chat"
}
```

## URL `/chat/make_manager/<str:chat_id>/<str:user_id>/`

Description: Promotes a user to manager role in a chat.

### POST

Description: Gives a user manager privileges in a chat. Only the owner can promote users.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User or group chat does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User does not have permissions"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "User is already a manager"
}
```

## URL `/chat/remove_manager/<str:chat_id>/<str:user_id>/`

Description: Demotes a user from manager role in a chat.

### POST

Description: Removes manager privileges from a user. Only the owner can demote managers.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User or group chat does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User does not have permissions"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "User is not a manager"
}
```

## URL `/chat/transfer_ownership/<str:chat_id>/<str:user_id>/`

Description: Transfers chat ownership to another user.

### POST

Description: Makes another user the owner of the chat and demotes the current owner to manager.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User or group chat does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User does not have permissions"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "User is already the owner"
}
```

## URL `/chat/send_join_request/<str:chat_id>/<str:user_id>/`

Description: Sends a join request for a user to join a chat.

### POST

Description: Invites a friend to join a chat, creating a join request that managers can accept or reject.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "Inviter and target user are not friends"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "Target user is already in chat"
}
```

## URL `/chat/get_join_requests/<str:chat_id>/`

Description: Retrieves pending join requests for a chat.

### GET

Description: Returns all pending join requests for a chat. Only managers and owners can view join requests.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "join_requests": [
      {
        "id": 1,
        "user_id": 2,
        "username": "user2",
        "profile_picture": "http://example.com/media/profile/2.jpg",
        "timestamp": "2023-10-01T12:00:00Z"
      }
    ]
  }
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Group Chat does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User does not have permissions"
}
```

## URL `/chat/accept_join_request/<str:chat_id>/<str:user_id>/`

Description: Accepts a join request to a chat.

### POST

Description: Approves a join request, adding the user to the chat. Only managers and owners can accept join requests.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Target user does not have a join request"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User does not have permissions"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "Target user is already in chat"
}
```

## URL `/chat/reject_join_request/<str:chat_id>/<str:user_id>/`

Description: Rejects a join request to a chat.

### POST

Description: Declines a join request. Only managers and owners can reject join requests.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Target user does not have a join request"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User does not have permissions"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "Target user is already in chat"
}
```

## URL `/chat/remove_member/<str:chat_id>/<str:user_id>/`

Description: Removes a member from a chat.

### POST

Description: Removes a user from a chat. Owners can remove anyone except themselves, managers can only remove regular members.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "User does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "Insufficient permissions to remove members"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "Cannot remove the chat owner"
}
```

## URL `/chat/leave_chat/<str:chat_id>/`

Description: Leaves a chat as a member.

### POST

Description: Allows a user to leave a chat. Owners cannot leave without transferring ownership first.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {}
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "code": 2,
  "error": "Bad method"
}
```

**HTTP 401 Unauthorized**
```json
{
  "code": 2,
  "error": "Invalid JWT token"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Group Chat does not exist"
}
```

**HTTP 409 Conflict**
```json
{
  "code": 2,
  "error": "Chat owner cannot leave without transferring ownership first"
}
```