# UI Specification

## Web Pages

---

### 1. Dashboard (`/`)

**Purpose:** Central hub for matchmaking

| Section | Features |
|---------|----------|
| Quick Match | Platform, Nightlord (optional), Mode, [Find Match] |
| War Room | [Create Room], [Browse Rooms] |
| SOS | [Request Aid], [View Bounty Board] |
| Active Room | Sticky banner if user is in a room |

---

### 2. Room List (`/rooms`)

**Purpose:** Browse and join rooms

**Filters:** Platform, Nightlord, Mode, Depth, Region, Mic, Open/Closed

**Room Card:**
```
┌─────────────────────────────────────────────┐
│ Gladius                             2/3 ●●○ │
│ PC | DoN Depth 3 | EU | Mic                 │
│ Host: WarriorKing ⭐42                       │
│ "Looking for DPS, know the fight"           │
│ [Join]                          ⏱ 5 min    │
└─────────────────────────────────────────────┘
```

**Username Interaction:**
- Hover → Profile mini-card popup
- Click → Navigate to `/profile/:id`

---

### 3. Create Room (`/rooms/create`)

**Form:**

| Field | Type | Required |
|-------|------|:--------:|
| Platform | Dropdown | ✅ |
| Mode | Normal / Deep of Night | ✅ |
| Nightlord | Dropdown (hidden for DoN) | ✅ Normal |
| Depth | 1-5 (only for DoN) | ✅ DoN |
| Region | Any / EU / NA | ✅ |
| Party Size | 3 or 6 (Seamless) | ✅ |
| Room Type | Open / Closed | ✅ |
| Mic | Required / Optional | ✅ |
| Description | Text (300 char) | Optional |

---

### 4. Room View (`/rooms/:id`)

**Header:**
```
┌─────────────────────────────────────────────┐
│ Gladius (DoN Depth 3)            2/3 OPEN  │
│ PC | EU | Mic Required                      │
│ "Looking for DPS, know the fight"           │
└─────────────────────────────────────────────┘
```

**Member List (always visible):**
```
┌─────────────────────────────────────────────┐
│ MEMBERS (2/3)                               │
│                                             │
│ ● WarriorKing ⭐42 (Host)        ✅ Ready   │
│ ● Player_A ⭐18                  ⏳ Waiting │
│ ○ (waiting for player)                      │
└─────────────────────────────────────────────┘
```

**Host Actions:**
- Closed Room: Applicant cards with [Accept] [Decline]
- Full Room: [Lock Room] button
- [Edit] [Close]

**Member Actions:**
- [Ready] toggle
- [Leave]

**Locked State:**
```
┌─────────────────────────────────────────────┐
│ PASSWORD                                    │
│           Nx882              [📋 Copy]     │
├─────────────────────────────────────────────┤
│ [🔊 Join Voice]  [💬 Open Thread]          │
└─────────────────────────────────────────────┘
```

---

### 5. SOS Create (`/sos/create`)

| Field | Type |
|-------|------|
| Boss | Dropdown |
| Platform | Dropdown |
| Note | Text |
| Mic | Yes / No |

---

### 6. Bounty Board (`/sos/board`)

**SOS Card:**
```
┌─────────────────────────────────────────────┐
│ 🆘 Messmer                      ⏱ 3 min   │
│ PC | No mic                                │
│ "Can't get past phase 2..."                │
│ Caller: StuckPlayer ⭐5                    │
│                            [Assist 🛡️]     │
└─────────────────────────────────────────────┘
```

---

### 7. Profile (`/profile/:id`)

**Sections:**
- Header: Avatar, name, tier badge, sherpa badge
- Stats: Runs, completion %, sherpa count
- Recent Runs: Last 10

**Mini-card (hover popup):**
```
┌─────────────────────────────────┐
│ WarriorKing ⭐42               │
│ Legend | 🛡️ Sherpa             │
│ 96% completion | 24 assists    │
└─────────────────────────────────┘
```

---

## Discord Bot

---

### 1. Intro Embed (pinned in #lfg)

```
┌─────────────────────────────────────────────┐
│ 🎮 NIGHTREIGN MATCHMAKING — PC              │
├─────────────────────────────────────────────┤
│ Find teammates for your next expedition.   │
├─────────────────────────────────────────────┤
│ [🟣 Create Room]  [⚡ Quick Match]         │
│ [🆘 Request SOS]  [📖 Help]                │
└─────────────────────────────────────────────┘
```

---

### 2. Create Room Modal

Triggered by [🟣 Create Room]:

| Field | Type |
|-------|------|
| Mode | Dropdown |
| Nightlord | Dropdown |
| Depth | Dropdown (DoN only) |
| Region | Dropdown |
| Mic | Dropdown |
| Description | Text input |

---

### 3. Room Card (posted in #lfg)

```
┌─────────────────────────────────────────────┐
│ Nightreign LFG — DoN Depth 3               │
├─────────────────────────────────────────────┤
│ Host       @WarriorKing ⭐42               │
│ Target     Gladius                         │
│ Region     EU | Mic Required               │
│                                             │
│ Members    2/3                              │
│ • @WarriorKing ⭐42  ✅                    │
│ • @Player_A ⭐18     ⏳                    │
│                                             │
│ "Looking for DPS, know the fight"          │
├─────────────────────────────────────────────┤
│ [📣 Push]  [🔔 Notify Me]  [🛑 Close]      │
└─────────────────────────────────────────────┘
```

**Buttons:**

| Button | Who | Action |
|--------|-----|--------|
| 📣 Push | Host | Sends notification to #lfg + subscribers |
| 🔔 Notify Me | Anyone | Subscribe for DM on updates |
| 🛑 Close | Host | Close room |
| Join | Anyone | Join room (Open rooms) |
| Apply | Anyone | Apply modal (Closed rooms) |

---

### 4. SOS Card (in #sos-requests)

```
┌─────────────────────────────────────────────┐
│ 🆘 SOS — Messmer                            │
├─────────────────────────────────────────────┤
│ "Can't get past phase 2..."                │
│ Caller: @StuckPlayer                        │
│ Posted: 3 min ago                           │
├─────────────────────────────────────────────┤
│              [Assist 🛡️]                   │
└─────────────────────────────────────────────┘
```

---

### 5. DM Notifications

**Match Found:**
```
┌─────────────────────────────────────────────┐
│ 🎮 MATCH FOUND                              │
├─────────────────────────────────────────────┤
│ Target: Gladius                             │
│ Password: Nx882           [📋 Copy]        │
│                                             │
│ • @WarriorKing (Host) ⭐42                 │
│ • @Player_A ⭐18                           │
│ • You                                       │
├─────────────────────────────────────────────┤
│ [🔊 Join Voice]  [💬 Open Thread]          │
└─────────────────────────────────────────────┘
```

**Ready Check:**
```
┌─────────────────────────────────────────────┐
│ ⏰ READY CHECK                              │
├─────────────────────────────────────────────┤
│ • @WarriorKing ✅                          │
│ • @Player_A ⏳                             │
│ • You ⏳                                   │
├─────────────────────────────────────────────┤
│      [✅ Ready]      [❌ Leave]            │
└─────────────────────────────────────────────┘
```

---

## Temporary Channels

When room locks:

| Type | Name | Visibility | Lifecycle |
|------|------|------------|-----------|
| Voice | `{Boss}-{Password}` | Party only | Delete 5 min after empty |
| Thread | `{Boss} \| {Password}` | Party only | Archive after 24h |

---

## Problem → Solution

| # | Problem | Solution |
|---|---------|----------|
| P1 | Duplicate listings | Live updates, expiry |
| P2 | Ambiguous slots | `2/3 ●●○` display |
| P3 | Voice friction | Auto-create private VC |
| P4 | Boss name variants | Dropdown selectors |
| P5 | No skill match | Rep display, filters |
| P6 | No playstyle signals | Vibe, Mic fields |
| P7 | Ephemeral threading | Room-based structure |
| P8 | No lifecycle | States, auto-close |
| P9 | Hidden password | Prominent display |
| P10 | Time blindness | Expiry timers |
| P11 | No reputation | Badges, stats |
