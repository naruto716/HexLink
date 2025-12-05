# User Journey Design

## Nightreign Gameplay Context

```
┌─────────────────────────────────────────────────────────────────────┐
│                    NIGHTREIGN RUN STRUCTURE                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  NIGHT 1                    NIGHT 2                    NIGHT 3      │
│  ┌─────────────────┐       ┌─────────────────┐       ┌───────────┐ │
│  │ Explore map     │       │ Explore map     │       │           │ │
│  │ (shrinking zone)│  ──►  │ (shrinking zone)│  ──►  │ NIGHTLORD │ │
│  │ Collect loot    │       │ Collect loot    │       │   BOSS    │ │
│  │ Level up        │       │ Level up        │       │           │ │
│  ├─────────────────┤       ├─────────────────┤       └───────────┘ │
│  │ NIGHT BOSS      │       │ NIGHT BOSS      │                     │
│  └─────────────────┘       └─────────────────┘                     │
│                                                                     │
│  Total run: 45-60 minutes                                          │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Platform Matrix

| Platform | Max Players | Notes |
|----------|:-----------:|-------|
| PC (Native) | 3 | Standard |
| PlayStation | 3 | Standard |
| Xbox | 3 | Standard |
| PC (Seamless Coop) | 6 | Mod-based |

---

## Room Model

| Room Type | Description |
|-----------|-------------|
| **Open Room** | Anyone can join instantly |
| **Closed Room** | Host approves each applicant |

---

## Journey 1: Quick Match

```mermaid
flowchart TD
    A[Quick Match] --> B[Select Platform]
    B --> C[Select Nightlord - optional]
    C --> D{Open Room available?}
    D -->|Yes| E[Join existing Open Room]
    D -->|No| F[System creates Open Room]
    E --> G[Room View: Password + VC]
    F --> G
    G --> H[Play]
```

---

## Journey 2: War Room — Host

### Closed Room (Manual Approval)

```mermaid
flowchart TD
    A[Create Room] --> B[Select Platform + Party Size]
    B --> C[Select Nightlord]
    C --> D[Set: Closed Room]
    D --> E[Room Created]
    E --> F[Review Applicants]
    F --> G{Accept?}
    G -->|Yes| H[Slot fills]
    G -->|No| F
    H --> I{Full?}
    I -->|No| F
    I -->|Yes| J[Ready Check → Lock]
    J --> K[Password + VC]
```

### Open Room (Auto-Fill)

```mermaid
flowchart TD
    A[Create Room] --> B[Set: Open Room]
    B --> C[Room Created]
    C --> D[Players join automatically]
    D --> E{Full?}
    E -->|No| D
    E -->|Yes| F[Ready Check → Lock]
    F --> G[Password + VC]
```

---

## Journey 3: War Room — Seeker

```mermaid
flowchart TD
    A[Browse Room List] --> B[Filter: Platform/Boss/Vibe]
    B --> C{Room Type?}
    C -->|Open| D[Click Join → Instant]
    C -->|Closed| E[Click Apply → Wait]
    D --> F[In Room]
    E --> G{Accepted?}
    G -->|Yes| F
    G -->|No| B
    F --> H[Ready Up → Play]
```

**Note:** Seekers can apply to multiple Closed Rooms simultaneously.

---

## Journey 4: SOS — Caller

```mermaid
flowchart TD
    A[Request Aid] --> B[Select Nightlord/Boss]
    B --> C[Add Notes]
    C --> D[Submit → Posted to Bounty Board]
    D --> E{Sherpa responds?}
    E -->|Yes| F[2/3 Open Room created]
    F --> G[Room in list, 3rd joins]
    G --> H[Ready Check → Play]
```

---

## Journey 5: SOS — Sherpa

```mermaid
flowchart TD
    A[View Bounty Board] --> B[Filter by Boss]
    B --> C[Select SOS Request]
    C --> D[Click Assist]
    D --> E[2/3 Open Room created with Caller]
    E --> F[3rd joins from room list]
    F --> G[Ready + Play]
    G --> H[Completion: +Reputation]
```

---

## UI Components

### Room Card

```
┌────────────────────────────────────────────────────────┐
│ 🔓 Gladius                                  2/3 ●●○   │
│ PC | Chill | No Mic              Host: WarriorKing    │
│ Open Room                          [Join Instantly]   │
└────────────────────────────────────────────────────────┘
```

### Match View (Password Display)

```
┌────────────────────────────────────────────────────────┐
│                  🎮 READY TO PLAY                      │
│                                                        │
│  PASSWORD                                              │
│  ┌──────────────────────────────────────────────────┐ │
│  │                    Nx882                         │ │
│  └──────────────────────────────────────────────────┘ │
│                    [📋 Copy]                          │
│                                                        │
│            [🔊 Join Voice Channel]                    │
│                                                        │
│  TEAM: WarriorKing • Player_A • You                   │
└────────────────────────────────────────────────────────┘
```

---

## Summary

```
┌─────────────────────────────────────────────────────────────────────┐
│                           DASHBOARD                                 │
│                                                                     │
│   ┌───────────────┐  ┌───────────────┐  ┌───────────────┐          │
│   │ QUICK MATCH   │  │   WAR ROOM    │  │      SOS      │          │
│   │               │  │               │  │               │          │
│   │ Auto-join or  │  │ [Create Room] │  │ [Request Aid] │          │
│   │ create Open   │  │ ├─ Open       │  │ [Bounty Board]│          │
│   │ Room          │  │ └─ Closed     │  │               │          │
│   │               │  │ [Browse Rooms]│  │               │          │
│   └───────────────┘  └───────────────┘  └───────────────┘          │
│                              │                                      │
│                              ▼                                      │
│                    ┌─────────────────┐                             │
│                    │ ROOM → Password │                             │
│                    │      → VC       │                             │
│                    │      → Play     │                             │
│                    └─────────────────┘                             │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```
