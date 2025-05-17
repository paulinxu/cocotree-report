```mermaid
classDiagram
    %% Group related models into sections using notes
    
    note "Authentication Models" as AuthSection
    note "Chat Models" as ChatSection
    note "Messaging Models" as MsgSection
    note "Friend Management Models" as FriendSection
    
    %% User Model
    class User {
        +BigAutoField id
        +CharField username
        +CharField password
        +EmailField email
        +PhoneNumberField phone_number
        +FloatField created_time
        +BooleanField is_active
        +ImageField profile_picture
        +TextField bio
        +deactivate()
        +set_password(raw_password)
        +check_password(raw_password)
        +create_user(username, password, email, phone_number)
    }
    
    %% Chat Management Models
    class Chat {
        +BigAutoField id
        +CharField chat_name
        +FloatField created_time
        +BooleanField private
        +ImageField chat_profile_picture
        +serialize()
        +refresh_profile_picture()
    }
    
    class ChatMember {
        +BigAutoField id
        +ForeignKey chat
        +ForeignKey user
        +FloatField last_access_time
        +ForeignKey latest_message
        +IntegerField unread_messages_count
        +BooleanField muted
        +BooleanField pinned
        +BooleanField has_messages
        +BooleanField is_deleted
        +CharField role
    }
    
    class JoinRequest {
        +BigAutoField id
        +ForeignKey chat
        +ForeignKey user
        +FloatField created_time
        +serialize()
    }
    
    %% Messaging Models
    class Message {
        +BigAutoField id
        +ForeignKey chat
        +ForeignKey user
        +CharField content
        +FileField attachment
        +FloatField timestamp
        +CharField message_type
        +IntegerField reply_count
        +BigIntegerField reply_id
    }
    
    class Announcement {
        +BigAutoField id
        +ForeignKey chat
        +CharField content
        +FloatField timestamp
        +serialize()
    }
    
    %% User Management Models
    class FriendRequest {
        +ForeignKey from_user
        +ForeignKey to_user
        +CharField status
        +DateTimeField last_updated
    }
    
    class Friendship {
        +ForeignKey user
        +ForeignKey friend
        +CharField group
        +DateTimeField created_at
    }
    
    %% Place models near their section notes
    AuthSection .. User
    
    ChatSection .. Chat
    ChatSection .. ChatMember
    ChatSection .. JoinRequest
    
    MsgSection .. Message
    MsgSection .. Announcement
    
    FriendSection .. FriendRequest
    FriendSection .. Friendship
    
    %% Relationships using simpler notation
    User "1" --> "many" Message: sends
    User "1" --> "many" FriendRequest: requests
    User "1" --> "many" FriendRequest: receives
    User "1" --> "many" Friendship: has
    User "1" --> "many" Friendship: is friend of
    User "1" --> "many" ChatMember: belongs to
    User "1" --> "many" JoinRequest: requests to join
    
    Chat "1" --> "many" Message: contains
    Chat "1" --> "many" ChatMember: has
    Chat "1" --> "many" JoinRequest: receives
    Chat "1" --> "many" Announcement: has
    
    Message "1" --> "many" ChatMember: latest message of
``` 