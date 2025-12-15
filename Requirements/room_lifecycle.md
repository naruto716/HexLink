# Room Lifecycle Design: VC-as-Presence Model (v2)

## Core Principles

> **VC presence = Room membership**
> - Join room → 5min to connect to VC or auto-kicked
> - In VC = in room
> - Leave VC = grace period → kicked if not rejoined
> - VC empty = room closed

No separate threads - use VC's built-in text chat.

---

## Room Modes

| Mode | Description | Who Can Join |
|------|-------------|--------------|
| **OPEN** | Anyone can join instantly | Anyone |
| **APPLY** | Host reviews and accepts/rejects | Must apply, host decides |
| **RUN** | Run in progress | No one (in-game) |
| **LOCKED** | Host manually locked the room | No one |

---

## Room Sizes

| Platform | Max Players |
|----------|:-----------:|
| PC / PlayStation / Xbox | 3 |
| PC (Seamless Co-op Mod) | 6 |

---

## State Machine

```mermaid
stateDiagram-v2
    [*] --> CREATED: Host creates room
    CREATED --> OPEN: Host joins VC
    CREATED --> APPLY: Host joins VC (apply mode)
    
    OPEN --> OPEN: Players join/leave
    APPLY --> APPLY: Players apply, host accepts/rejects
    
    OPEN --> RUN: Host starts run
    APPLY --> RUN: Host starts run
    OPEN --> LOCKED: Host locks room
    APPLY --> LOCKED: Host locks room
    
    RUN --> OPEN: Run ends (room reopens)
    LOCKED --> OPEN: Host unlocks
    LOCKED --> CLOSED: Host closes room
    
    OPEN --> EXPIRED: No host in VC for 10min
    APPLY --> EXPIRED: No host in VC for 10min
    CREATED --> EXPIRED: Host never joins VC
    
    OPEN --> CLOSED: Host closes room
    APPLY --> CLOSED: Host closes room
    
    CLOSED --> [*]
    EXPIRED --> [*]
```

---

## Join Flow: OPEN Room

```mermaid
sequenceDiagram
    participant Player
    participant Web
    participant Backend
    participant Bot
    participant Discord

    Player->>Web: Click "Join Room"
    Web->>Backend: POST /rooms/{id}/join
    Backend->>Backend: Add player to room (PENDING)
    Backend->>Backend: Start 5min VC grace timer
    Backend->>Web: Room joined + VC link
    Backend->>Bot: Update room card
    Bot->>Discord: Update embed
    Web->>Player: "Join VC within 5 min!"
    
    alt Player joins VC in time
        Player->>Discord: Joins VC
        Discord->>Bot: on_voice_state_update
        Bot->>Backend: VoiceJoin event
        Backend->>Backend: Player → READY, cancel timer
        Backend->>Web: Update room state
        Backend->>Bot: Update room card
    else 5min expires
        Backend->>Backend: Remove player from room
        Backend->>Web: Player kicked notification
        Backend->>Bot: Update room card
    end
```

---

## Join Flow: APPLY Room

```mermaid
sequenceDiagram
    participant Player
    participant Web
    participant Backend
    participant Bot
    participant Discord
    participant Host

    Player->>Web: Click "Apply"
    Web->>Backend: POST /rooms/{id}/apply
    Backend->>Backend: Add to applicants list
    Backend->>Web: "Application sent!"
    Backend->>Bot: Notify host
    Bot->>Discord: DM host: "New applicant!"
    
    Host->>Web: View applicants
    Web-->>Host: Show applicant profiles
    
    alt Host accepts
        Host->>Web: Click "Accept"
        Web->>Backend: POST /rooms/{id}/accept/{playerId}
        Backend->>Backend: Move to room (PENDING)
        Backend->>Backend: Start 5min VC grace timer
        Backend->>Web: Notify player: "Accepted!"
        Backend->>Bot: DM player with VC link
        Note over Player: Same VC join flow as OPEN
    else Host rejects
        Host->>Web: Click "Reject"
        Web->>Backend: POST /rooms/{id}/reject/{playerId}
        Backend->>Backend: Remove from applicants
        Backend->>Web: Notify player: "Rejected"
    end
```

---

## Run Lifecycle

```mermaid
sequenceDiagram
    participant Host
    participant Web
    participant Backend
    participant Bot
    participant Discord
    participant Players

    Note over Host,Players: === START RUN ===
    
    Host->>Web: Click "Start Run"
    Web->>Backend: POST /rooms/{id}/start
    Backend->>Backend: Room → LOCKED
    Backend->>Backend: Generate password
    Backend->>Web: Update all clients
    Backend->>Bot: Send password to VC chat
    Bot->>Discord: Post in VC: "Password: Nx882"
    Backend->>Bot: DM all players
    Bot->>Discord: DM each: "Run started! Password: Nx882"
    
    Note over Host,Players: === PLAYING ===
    Note right of Backend: Track run start time
    
    Note over Host,Players: === END RUN ===
    
    Host->>Web: Click "End Run"
    Web->>Backend: POST /rooms/{id}/end
    Backend->>Backend: Calculate run duration
    
    alt Run >= 10 min
        Backend->>Backend: Unlock review eligibility
        Backend->>Bot: DM all: "Rate teammates?"
    else Run < 10 min
        Note right of Backend: No review eligibility
    end
    
    Backend->>Backend: Room → OPEN (or APPLY)
    Backend->>Web: Update all clients
    Backend->>Bot: Update room card
    
    Note over Host,Players: Room reopens for more runs!
```

---

## Grace Period (All States)

Every player in the room must stay in VC. Grace period applies universally.

```mermaid
flowchart TD
    A[Player joins room] --> B[5min to join VC]
    B --> C{Joins VC?}
    C -->|Yes| D[Player is READY ✅]
    C -->|No, 5min| E[Auto-kicked from room]
    
    D --> F[Player in room]
    F --> G[Player disconnects from VC]
    G --> H[5min grace period starts]
    H --> I{Rejoins VC?}
    I -->|Yes| F
    I -->|No, 5min| J[Auto-kicked from room]
    
    style B fill:#ff9
    style H fill:#ff9
```

---

## Host Actions

| Action | Endpoint | Effect |
|--------|----------|--------|
| **Start Run** | `POST /rooms/{id}/start` | Lock room, send password |
| **End Run** | `POST /rooms/{id}/end` | Unlock room, prompt reviews if eligible |
| **Kick Player** | `POST /rooms/{id}/kick/{userId}` | Remove player immediately |
| **Close Room** | `DELETE /rooms/{id}` | Delete room and VC |
| **Change Mode** | `PATCH /rooms/{id}` | Switch between OPEN/APPLY |

---

## Leave Room & Host Transfer

```mermaid
flowchart TD
    A[Player clicks Leave] --> B{Is host?}
    B -->|No| C[Remove from room]
    B -->|Yes| D{Other players?}
    D -->|Yes| E[Promote oldest member to host]
    D -->|No| F[Close room]
    
    E --> G[Notify new host]
    G --> H[Continue room]
    
    C --> I[Update room card]
    F --> I
    H --> I
```

---

## Review System

### Eligibility

| Condition | Rule |
|-----------|------|
| Run duration | Must be ≥ 10 minutes |
| Same run | Must have been in the same run together |
| Per run | Each user can rate another user **once per valid run** |

### Flow

```mermaid
sequenceDiagram
    participant Backend
    participant Bot
    participant Discord
    participant User
    participant Web

    Backend->>Backend: Run ends, duration ≥ 10min
    Backend->>Backend: Create pending reviews for run
    Backend->>Bot: Send review prompt
    Bot->>Discord: DM: "Rate your teammates! [Review on Web]"
    User->>Discord: Clicks link
    Discord->>Web: Magic link to /reviews/{runId}
    Web->>User: Show teammates with 5-star rating
    User->>Web: Submit ratings (1-5 stars each)
    Web->>Backend: POST /reviews
    Backend->>Backend: Store ratings for this run
```

### Web Rating UI

| Element | Description |
|---------|-------------|
| Teammate cards | Avatar, name, reputation tier |
| 5-star rating | Click to rate 1-5 stars |
| Optional comment | Text field (optional) |
| Skip button | Skip rating this player |

---

## Update Targets

When room state changes, update BOTH:

| Target | Method |
|--------|--------|
| **Web** | WebSocket push to all connected clients |
| **Discord** | Bot updates embed in #lfg |

---

## API Endpoints

### Room Management
| Endpoint | Action |
|----------|--------|
| `POST /rooms` | Create room |
| `GET /rooms` | List active rooms |
| `GET /rooms/{id}` | Room details |
| `PATCH /rooms/{id}` | Update room settings |
| `DELETE /rooms/{id}` | Close room |

### Player Actions
| Endpoint | Action |
|----------|--------|
| `POST /rooms/{id}/join` | Join open room |
| `POST /rooms/{id}/leave` | Leave room |
| `POST /rooms/{id}/apply` | Apply to room |

### Host Actions
| Endpoint | Action |
|----------|--------|
| `POST /rooms/{id}/accept/{userId}` | Accept applicant |
| `POST /rooms/{id}/reject/{userId}` | Reject applicant |
| `POST /rooms/{id}/kick/{userId}` | Kick player |
| `POST /rooms/{id}/start` | Start run |
| `POST /rooms/{id}/end` | End run |

### Ratings
| Endpoint | Action |
|----------|--------|
| `POST /ratings` | Submit rating |
| `GET /ratings/{userId}` | Get user's rating summary |

---

## gRPC Events (Bot ↔ Backend)

| Direction | Event | Trigger |
|-----------|-------|---------|
| Backend → Bot | `CreateVC` | Room created |
| Bot → Backend | `VoiceJoin` | User joins VC |
| Bot → Backend | `VoiceLeave` | User leaves VC |
| Backend → Bot | `UpdateRoomCard` | Room state changes |
| Backend → Bot | `SendDM` | Notifications |
| Backend → Bot | `PostToVC` | Send message to VC chat |
| Backend → Bot | `DeleteVC` | Room closes |
