# Messages API API Documentation

Group: Cocotree

## URL `messages/send_message/<str:chat_id>/`

Description: Sends message to a chat.

### POST

Description: Creates text or file message in a specific chat.

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{
  "content": "Hello, world!",
  "attachment": "file.jpg",
  "reply_id": 1,
  "message_type": "text"
}
```
Note: message_type can be "text", "image", "file", or "voice". reply_id refers to the message the current message is replying to

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "message sent successfully",
    "data": {
      "message_id": 1,
      "sender_id": 1,
      "reply_id": 1,
      "reply_count": 0,
      "message_type": "text",
      "content": "Hello, world!",
      "timestamp": 1234567890,
      "file_url": "/media/chat_files/file.jpg"
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
  "error": "Invalid token"
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

**HTTP 403 Forbidden (Private Chat)**
```json
{
  "code": 1,
  "error": "You are not friends with username"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Error message from serializer"
}
```

## URL `messages/get_messages/<str:chat_id>/<str:message_id>/<str:get_after>/<str:count>/`

Description: Retrieves chat messages.

### GET

Description: Gets messages from a chat, with pagination based on message_id.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Request Parameters
- chat_id: ID of the chat
- message_id: Starting message ID (-1 for most recent messages)
- get_after: "true" to get messages after message_id, "false" to get messages before
- count: Number of messages to retrieve

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "messages": [
      {
        "message_id": 1,
        "sender_id": 1,
        "reply_id": 1,
        "reply_count": 0,
        "content": "Hello, world!",
        "message_type": "text",
        "timestamp": 1234567890,
        "file_url": "/media/chat_files/file.jpg"
      }
    ]
  }
}
```

#### Error Responses

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
  "error": "User not in chat"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 2,
  "error": "Invalid count value"
}
```

## URL `messages/filter_messages/<str:chat_id>/`

Description: Filters messages in a chat based on various criteria.

### GET

Description: Searches/filters messages by sender, type, content, and timestamp range.

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{
  "sender_id": 1, // optional
  "message_type": "text", // optional
  "content": "hello", // optional
  "start_timestamp": 1234567890, // optional
  "end_timestamp": 2234567890, // optional
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "messages": [
      {
        "message_id": 1,
        "sender_id": 1,
        "reply_id": 1,
        "reply_count": 0,
        "content": "Hello, world!",
        "message_type": "text",
        "timestamp": 1234567890,
        "file_url": "/media/chat_files/file.jpg"
      }
    ]
  }
}
```

#### Error Responses

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

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Invalid start_timestamp format"
}
```

## URL `messages/delete_message/<str:chat_id>/<str:message_id>/`

Description: Deletes a message from a chat.

### DELETE

Description: Marks a message as recalled (content replaced with system message).

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Request Parameters
- chat_id: ID of the chat
- message_id: ID of the message to delete

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "message": "message deleted successfully",
    "data": {
      "message_id": 1,
      "sender_id": 1,
      "content": "[message recalled by username]",
      "message_type": "system",
      "timestamp": 1234567890
    }
  }
}
```

#### Error Responses

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Chat does not exist"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Message does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 1,
  "error": "User is not a member of this chat"
}
```

## URL `messages/send_announcement/<str:chat_id>/`

Description: Sends an announcement to a chat.

### POST

Description: Creates an announcement in a group chat (owner/manager only).

#### Request Header Format
- Content-Type: application/json
- Authorization: <JWT_TOKEN>

#### Request Body Format
```json
{
  "content": "Announcement content"
}
```

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "id": 1,
    "chat_id": 1,
    "content": "Announcement content",
    "timestamp": 1234567890
  }
}
```

#### Error Responses

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
  "error": "User does not have permissions to send announcement"
}
```

**HTTP 400 Bad Request**
```json
{
  "code": 1,
  "error": "Content is required"
}
```

## URL `messages/delete_announcement/<str:chat_id>/<str:announcement_id>/`

Description: Deletes an announcement from a chat.

### DELETE

Description: Removes an announcement from a group chat (owner/manager only).

#### Request Header Format
- Authorization: <JWT_TOKEN>

#### Request Parameters
- chat_id: ID of the chat
- announcement_id: ID of the announcement to delete

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "id": 1,
    "chat_id": 1,
    "content": "Announcement content",
    "timestamp": 1234567890
  }
}
```

#### Error Responses

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "GroupChat does not exist"
}
```

**HTTP 404 Not Found**
```json
{
  "code": 2,
  "error": "Announcement does not exist"
}
```

**HTTP 403 Forbidden**
```json
{
  "code": 2,
  "error": "User does not have permissions to delete announcement"
}
```