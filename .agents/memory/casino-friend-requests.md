---
name: Casino friend-request architecture
description: Why friend requests use /friendRequests/{uid}/ instead of /users/{uid}/friendRequests/
---

Friend requests are stored at `/friendRequests/{targetUid}/{reqId}` (top-level path), not inside `/users/{targetUid}/friendRequests/`.

**Why:** Firebase rules that restrict `/users/$uid/.write` to `auth.uid === $uid` would block any user from sending a request to another user's node. The top-level path allows `write: auth != null` (any auth user can write incoming requests) while the requester's outgoing tracking goes into their own `/users/{uid}/sentRequests/`.

**Security guard:** When an `accepted` event arrives at `/friendRequests/{myUid}/`, the code validates that `sentRequests/{origReqId}.to === d.from` before adding the friend, preventing forged acceptance events.

**How to apply:** Any new cross-user notification (room invites, etc.) should follow the same pattern: top-level `/notifs/{targetUid}/` rather than writing into `/users/{targetUid}/`.
