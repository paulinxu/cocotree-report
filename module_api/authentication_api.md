# Authentication API Documentation

Group: Cocotree

## URL `/auth/register`

Description: User registration endpoint for creating new accounts

### POST

Description: Creates a new user account and returns a JWT token

#### Request Header Format
- Content-Type: application/json

#### Request Body Format
```json
{
  "username": "string",       // Required: 1-20 chars, alphanumeric/+_.-
  "password": "string",       // Required: Encrypted, 8+ chars, 1+ digit, 1+ uppercase
  "email": "string",          // Optional
  "phone_number": "string",   // Optional
  "profile_picture": "file",  // Optional
  "bio": "string"             // Optional
}
```
Note: The password must be encrypted with AES using the shared key before sending.

#### Successful Response
```json
{
  "code": 0,
  "payload": {
    "token": "string",  // JWT authentication token
    "id": integer       // User ID
  }
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "error": "HTTP method 'GET' is not allowed on this endpoint.",
  "code": 2
}
```

**HTTP 400 Bad Request - Password Decryption Error**
```json
{
  "error": "An error occured while handling password",
  "code": 2
}
```

**HTTP 400 Bad Request - Validation Error**
```json
{
  "error": "Username already exists.",
  "payload": {
    "username": [
      "Username already exists."
    ],
  },
  "code": 1
}
```

## URL `/auth/login`

Description: Authentication endpoint that validates user credentials and returns a JWT token upon success.

### POST

Description: Validates user credentials and returns JWT token for authenticated users.

#### Request Header Format
- Content-Type: application/json

#### Request Body Format
```json
{
  "username": "string",
  "password": "string"
}
```
Note: The password must be encrypted with AES using the shared key before sending.

#### Successful Response
```json
{
  "success": true,
  "payload": {
    "token": "string",
    "id": "number"
  },
  "status": 200
}
```

#### Error Responses

**HTTP 405 Method Not Allowed**
```json
{
  "success": false,
  "message": "Method not allowed",
  "status": 405
}
```

**HTTP 401 Unauthorized**
```json
{
  "success": false,
  "message": "User does not exist",
  "code": 1,
  "status": 401
}
```

OR

```json
{
  "success": false,
  "message": "Wrong password",
  "code": 1,
  "status": 401
}
```

**HTTP 400 Bad Request**
```json
{
  "success": false,
  "message": "An error occured while handling password",
  "code": 2,
  "status": 400
}
```

OR

```json
{
  "success": false,
  "message": "Missing or error type of [username]",
  "status": 400
}
```

OR

```json
{
  "success": false,
  "message": "Missing or error type of [password]",
  "status": 400
}
```

