# User story

The following storyline can be interpreted to be in chronological order.

## Part 1: User management

On the login page, users are able to choose between two options: "sign in" or "create a new account". 

When creating a new account, a new account, the user has to choose a username (under 20 characters), email (valid email address format), and password (at least 8 characters; contains an uppercase character and a number). The username and email must be unique.

Alternatively, the user chooses to sign into an existing account. 

AES encryption and decryption is used when sending the password from the front end to the backend. The password is the hashed before storing into the database.

After a successful sign in or registration, the backend returns a JWT token that expires after 24h, which the front end stores in cookies, so that the user gets automatically logged in the next time. 

User exclusive websocket channels are be created to instantly obtain information about new friend requests, messages, announcements, or changes to chats.  

In the user settings page, the user can update the following account information: username, email, password, profile picture, phone number, and biography. Updating the password, email, and phone number require password verification. The user can also log out or delete his/her account. In the latter case, the user is logged out and permanently loses access to the account. Other users will see the deleted status, and private chats with the deleted user will enter a read-only "archive" mode.

On the friends page, the user can see its list of friends, search friends by prefix, and view their profile information. For non-friend searches, the target username must perfectly match, enhancing the overall privacy of our userbase. If the search is successful, a friend request can be sent and the target user will be notified through websocket if they have an active session.

Once the target user accepts the friend request, these two users become friends, creating a private chat between them. New friends have tag "default", which can be changed on the friends list page. 

When a user deletes a friend, the private chat between them is also deleted.

## Part 2: Chat Management

The user can select a list of friends and create a group chat (needs a chat name). 

Within a chat, the information section displays the chat's name, picture, members (with their respective roles), and announcements. 

Only owner is able to add and remove managers, and can transfer ownership to other members. The original owner becomes a manager. The owner and managers are able to send announcements, which are notified by websocket to all members of the group.

All users in a chat can invite friends. Invitations have to be accepted by the owner or a manager. A user can only kick users that have strictly lower hierarchy. The owner of a chat must transfer ownership before leaving, while all other users can leave at any time. 

When a user joins a chat, leaves a chat, has a role change, or when the chat is deleted, all members in a chat are notified via web socket. When there is a new invite, the owner and managers are notified. 

## Part 3: Messaging

In a chatbox, the user can scroll up to view all messages (messages are obtained in batches reduce the stress on the frontend as chats get longer). Each message is accompanied by a username and profile picture. The user's own messages are displayed on the right hand side in blue, while all other messages are on the left hand side. 

When sending a message, the user can attach files, images, and voice recordings on top of the text. 

By keeping track of the last time a user fetched a message from the chat, the app is able to display the read status of all members in the chat. In a similar way, when displaying a user's list of chats, the app calculates the number of unread messages for each chat.

By right clicking onto a message, the send time is displayed and a list of actions pops up. The user can reply or delete a message. If a message is a reply, it can jump to the message it is replying to. Messages with replies show a reply count. 

Users can query messages in chats by file, image, member, and time. 

Users can also mute (disable notifications) and pin (set at the top of the list) chats.

When a message is sent, all users in a chat are notified via websocket. 

## Part 4: Additional features

Users can start video call in chats. In a video call, the microphone and camera can be turned off at any time. 
