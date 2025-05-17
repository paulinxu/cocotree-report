# coco-backend description

The coco-backend is built using Django and Django REST Framework with WebSocket support through Django Channels.

## Applications

Authentication: Handles user registration and login using JWT tokens for secure authentication. Passwords are encrypted and securely stored.

User Management: Manages user profiles, friend relationships, and user-related interactions. It includes WebSocket consumers for real-time user status updates.

Messaging: Provides functionality for real-time messaging with WebSocket support. It handles message creation, retrieval, and notifications through Django Channels.

Chat Management: Manages group chats, chat members, and announcements. It coordinates with messaging to handle chat-related operations.

Calling: Implements voice and video calling features using WebRTC technology over WebSockets.

## Backend Architecture

Django for the core web framework and ORM\
Django REST Framework for API endpoints\
Django Channels with Redis for WebSocket support\
JWT for authentication\
MySQL for production database (SQLite for development/testing)\
Daphne as the ASGI server

## The WebSocket implementation

Redis is used as a channel layer for WebSocket communication, ensuring efficient real-time messaging across multiple instances of the application.

We implement websocket so that:

Friends requests notify the destination user\
Messages are delivered instantly\
User status updates are pushed in real-time\
Announcements are pushed to users immediately
