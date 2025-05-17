# API Summary

## Authentication API
- **Register**: `/auth/register` - Creates a new user account and returns JWT token
- **Login**: `/auth/login` - Validates user credentials and returns JWT token

[Authentication Appendix](authentication_api.md)

## User Management API
- **User Info**: `/users/user_info/<id>/` - Retrieves basic user information by user ID
- **Search Users**: `/users/search_users/` - Searches for users by exact username match
- **Search Friends**: `/users/search_friends/` - Searches for friends with optional filtering by username prefix/group
- **User Details**: `/users/user_details/` - Gets/updates current user's profile information
- **User Details Sensitive**: `/users/user_details/sensitive/` - Manages sensitive user information
- **Delete Account**: `/users/delete_account/` - Permanently deletes user account

### Friend Management
- **Send Friend Request**: `/users/friends/request/` - Sends a friend request to another user
- **Respond to Friend Request**: `/users/friends/respond/<request_id>/` - Accept/reject friend requests
- **Delete Friend**: `/users/friends/delete/<friend_id>/` - Removes a user from friends list
- **Friend Groups**: `/users/friends/groups/` - Manages friend grouping categories
- **Received Requests**: `/users/friends/received_requests/` - Lists pending friend requests received
- **Sent Requests**: `/users/friends/sent_requests/` - Lists pending friend requests sent
- **Update Friend Group**: `/users/friends/group_update/<friend_id>/` - Updates friend's group assignment

[User Management Appendix](user_management_api.md)

## Chat Management API
- **Chat List**: `/chat/chatlist/` - Lists all chats user is part of (ordered by pinned status and latest message)
- **Get Chat Info**: `/chat/get_chat_info/<chat_id>/` - Retrieves detailed chat information (members, announcements)
- **Create Chat**: `/chat/create_chat/` - Creates a new group chat with specified members
- **Delete Chat**: `/chat/delete_chat/<chat_id>/` - Deletes a chat
- **Toggle Mute**: `/chat/toggle_mute/<chat_id>/` - Toggles chat notification mute status
- **Toggle Pinned**: `/chat/toggle_pinned/<chat_id>/` - Toggles chat pinned status
- **Leave Chat**: `/chat/leave_chat/<chat_id>/` - Leaves a chat

### Chat Administration
- **Make Manager**: `/chat/make_manager/<chat_id>/<user_id>/` - Promotes user to chat manager role
- **Remove Manager**: `/chat/remove_manager/<chat_id>/<user_id>/` - Demotes user from chat manager role
- **Transfer Ownership**: `/chat/transfer_ownership/<chat_id>/<user_id>/` - Transfers chat ownership
- **Send Join Request**: `/chat/send_join_request/<chat_id>/<user_id>/` - Invites user to join chat
- **Get Join Requests**: `/chat/get_join_requests/<chat_id>/` - Lists pending chat join requests
- **Accept Join Request**: `/chat/accept_join_request/<chat_id>/<user_id>/` - Approves chat join request
- **Reject Join Request**: `/chat/reject_join_request/<chat_id>/<user_id>/` - Denies chat join request
- **Remove Member**: `/chat/remove_member/<chat_id>/<user_id>/` - Removes user from chat

[Chat Management Appendix](chat_management_api.md)

## Messages API
- **Send Message**: `/messages/send_message/<chat_id>/` - Creates text or file messages in chat
- **Get Messages**: `/messages/get_messages/<chat_id>/<message_id>/<get_after>/<count>/` - Retrieves chat messages with pagination
- **Filter Messages**: `/messages/filter_messages/<chat_id>/` - Searches/filters messages by sender, type, content, timestamp
- **Delete Message**: `/messages/delete_message/<chat_id>/<message_id>/` - Removes a message
- **Send Announcement**: `/messages/send_announcement/<chat_id>/` - Creates chat announcement
- **Delete Announcement**: `/messages/delete_announcement/<chat_id>/<announcement_id>/` - Removes chat announcement

[Messages Appendix](messages_api.md)

## Calling API
- **Send Invite**: `/calling/send_invite/` - Initiates call invitations

## WebSockets
- **Announcements**: `ws/announcements/` - Real-time chat announcements
- **Messages**: `ws/messages/` - Real-time messaging
- **Chat Management**: `ws/chat_management/` - Real-time chat updates
- **Call Invites**: `ws/call_invites/` - Real-time call invitation notifications
- **Callroom**: `ws/callroom/<room_id>/` - Real-time call room communication