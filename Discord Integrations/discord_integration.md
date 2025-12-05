# Discord Integration Requirements

## Platform Responsibilities

| Feature | Web | Discord |
|---------|:---:|:-------:|
| Browse Rooms | ✅ | - |
| Create Room | ✅ | ✅ |
| Quick Match | ✅ | ✅ |
| SOS Request | ✅ | ✅ |
| Bounty Board | ✅ | - |
| Notifications | - | ✅ |
| Voice Channel | - | ✅ |
| Room Chat | - | ✅ (thread) |

---

## Authentication: Magic Link

| Property | Value |
|----------|-------|
| Expiry | 5 minutes |
| Uses | Single-use |
| Tied to | Discord User ID |
| Fallback | OAuth for direct web visits |

---

## Bot Commands

### /quickmatch

```
Options:
├── Platform: PC, PlayStation, Xbox, Seamless
├── Nightlord: [dropdown, optional - default "Any"]
└── Mode: Normal, Deep of Night
```

### /createroom

```
Options:
├── Platform: PC, PlayStation, Xbox, Seamless
├── Nightlord: [dropdown, required for Normal mode]
├── Mode: Normal, Deep of Night
├── Depth: 1-5 (if Deep of Night)
├── Region: Any, EU, NA
├── Type: Open, Closed
├── Party Size: 3 (or up to 6 for Seamless)
└── Mic: Required, Optional
```

### /sos

```
Options:
├── Boss: [dropdown]
├── Note: Text input
└── Mic: Yes, No
```

### /browse

Returns magic link to web room list.

---

## Notifications (DMs)

### Match Found

```
┌────────────────────────────────────────────────────────────┐
│ 🎮 MATCH FOUND                                             │
│                                                            │
│ Target: Gladius                                            │
│                                                            │
│ PASSWORD                                                   │
│ ┌──────────────────────────────────────────────────────┐  │
│ │                      Nx882                           │  │
│ └──────────────────────────────────────────────────────┘  │
│                    [📋 Copy]                              │
│                                                            │
│ TEAM                                                       │
│ • @WarriorKing (Host) ⭐42                                │
│ • @Player_A ⭐18                                          │
│ • @You                                                    │
│                                                            │
│ [🔊 Join Voice]  [💬 Open Thread]                         │
└────────────────────────────────────────────────────────────┘
```

### Application Accepted

```
┌────────────────────────────────────────────────────────────┐
│ ✅ APPLICATION ACCEPTED                                    │
│                                                            │
│ Gladius (Deep Night) - WarriorKing                        │
│                                                            │
│ [🔊 Join Voice]  [💬 Open Thread]                         │
└────────────────────────────────────────────────────────────┘
```

### SOS Sherpa Response

```
┌────────────────────────────────────────────────────────────┐
│ 🛡️ HELP IS ON THE WAY                                     │
│                                                            │
│ VeteranHelper is coming to assist with Gladius!           │
│ Room created. Waiting for 1 more player.                  │
│                                                            │
│ [🔊 Join Voice]  [💬 Open Thread]                         │
└────────────────────────────────────────────────────────────┘
```

### Ready Check

```
┌────────────────────────────────────────────────────────────┐
│ ⏰ READY CHECK                                             │
│                                                            │
│ Room is full! Click Ready when you're at your PC.         │
│                                                            │
│ • @WarriorKing: ✅ Ready                                  │
│ • @Player_A: ⏳ Waiting                                   │
│ • @You: ⏳ Waiting                                        │
│                                                            │
│           [✅ Ready]        [❌ Leave]                    │
└────────────────────────────────────────────────────────────┘
```

### Room Locked (Final)

```
┌────────────────────────────────────────────────────────────┐
│ 🎮 ROOM READY — GO!                                        │
│                                                            │
│ PASSWORD: Nx882                    [📋 Copy]              │
│                                                            │
│ [🔊 Join Voice]  [💬 Open Thread]                         │
└────────────────────────────────────────────────────────────┘
```

---

## Channel Interface: Room Card

Posted in #lfg channel when room is created:

### Room Card Embed

```
┌────────────────────────────────────────────────────────────┐
│ Nightreign LFG — DoN Depth 3 (Trio)                       │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ Host           @WarriorKing ⭐42                          │
│ Target         Gladius                                    │
│ Region         EU • Mic Required                          │
│                                                            │
│ Status         🟡 ALMOST READY (2/3)                      │
│                                                            │
│ Members in voice:                                          │
│ • @WarriorKing ⭐42                                       │
│ • @Player_A ⭐18                                          │
│                                                            │
│ Notes          "Need DPS, know the fight"                 │
│                                                            │
│ ⏱ Last push: 2 min ago                                   │
├────────────────────────────────────────────────────────────┤
│ [📣 Push]  [🔔 Notify Me]  [🛑 Close]                     │
└────────────────────────────────────────────────────────────┘
```

### Status Progression

```
OPEN (0/3) → ALMOST (2/3) → FULL (3/3) → LOCKED → CLOSED
                                  ↓
                         2 min grace period
```

### Buttons

| Button | Who Can Use | Action |
|--------|-------------|--------|
| 📣 Push | Host only | Bumps visibility, 60s cooldown |
| 🔔 Notify Me | Anyone | Subscribe for DM when full |
| 🛑 Close | Host only | Closes the room |

---

## Push System

| Property | Value |
|----------|-------|
| Cooldown | 60 seconds per room |
| Effect | Bumps thread + channel visibility |
| Stale hint | Show "No push in: XX:XX" after 5 min |

---

## "Notify Me" Feature

1. User clicks [🔔 Notify Me] on room card
2. User ID added to subscribers list
3. When room fills (3/3), bot DMs all subscribers
4. Rate limit: Max 3 DMs per user per hour

---

## Temporary Channels

### Private Voice Channel

| Property | Value |
|----------|-------|
| Created | When room locks |
| Visibility | Only party members (permission overwrites) |
| Name | `{Boss}-{Password}` (e.g., Gladius-Nx882) |
| User limit | Party size (3 or 6) |
| Deleted | 5 min after empty |

### Private Thread

| Property | Value |
|----------|-------|
| Created | When room locks |
| Visibility | Only party members (private thread) |
| Name | `{Boss} | {Password}` |
| Auto-archive | 24 hours |
| Parent | #lfg-rooms channel |

### Creation Flow

```
Room locks → Create private VC → Create private thread
                    │                    │
                    └──────┬─────────────┘
                           ▼
              Post password + links in thread
                           │
                           ▼
              DM all players with same info
```

---

## Channel Posts

### #lfg Channel

Room cards posted here (Open rooms only):

```
┌────────────────────────────────────────────────────────────┐
│ 🔓 Gladius (DoN Depth 3)                        2/3 ●●○  │
│ PC | EU | Tryhard | Mic Req                               │
│ Host: @WarriorKing ⭐42                                   │
│                                        [Join] [Notify Me] │
└────────────────────────────────────────────────────────────┘
```

### #sos-requests Channel

SOS requests posted here:

```
┌────────────────────────────────────────────────────────────┐
│ 🆘 Messmer                                     ⏱ 3 min   │
│ "Can't get past phase 2..."                               │
│ Caller: @StuckPlayer                                      │
│                                            [Assist 🛡️]    │
└────────────────────────────────────────────────────────────┘
```

---

## Expiry System

| Trigger | Action |
|---------|--------|
| 40 min inactivity | Auto-expire room |
| All players leave | Close room |
| Host clicks Close | Close room |
| Sweeper | Runs every 2 min |

---

## Bidirectional Sync

| Action | Web → Discord | Discord → Web |
|--------|:-------------:|:-------------:|
| Create room | Post to #lfg | Show in room list |
| Join room | DM notification | Update slot count |
| Ready check | Update embed | Update status |
| Push | - | Bump visibility |
| Close | Delete/archive | Remove from list |

---

## Bot Permissions

```
SEND_MESSAGES
EMBED_LINKS
USE_SLASH_COMMANDS
MANAGE_CHANNELS (for temp VCs)
MANAGE_THREADS (for private threads)
CREATE_PRIVATE_THREADS
VIEW_CHANNEL
CONNECT
```
