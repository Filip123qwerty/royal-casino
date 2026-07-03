---
name: Casino Firebase Realtime Database required rules
description: Minimum Firebase RTDB rules for Royal Casino multiplayer features
---

The following rules must be pasted in Firebase Console → Realtime Database → Rules:

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "users": {
      ".read": "auth != null",
      "$uid": {
        ".write": "auth != null && auth.uid === $uid"
      },
      ".indexOn": ["nameLower", "ranking", "coins"]
    },
    "friendRequests": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null"
      }
    },
    "rooms": {
      ".read": "auth != null",
      ".write": "auth != null",
      ".indexOn": ["game", "code", "status"]
    },
    "matchmaking": {
      ".read": "auth != null",
      ".write": "auth != null",
      ".indexOn": ["game"]
    },
    "usernames": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

**Why:** Default Firebase rules block all writes. Rooms and matchmaking need open read/write for all auth users. Users node restricts writes to own data only. friendRequests allows any auth user to write (to send requests) but only the target to read.

**How to apply:** Go to Firebase Console → project → Realtime Database → Rules tab → paste → Publish.
