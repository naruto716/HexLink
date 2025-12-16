# User Stories: Room Matchmaking System

## Overview

User stories for the Nightreign matchmaking platform, grouped by page/feature. Each story follows the format:
> **As a** [user type], **I want** [action], **so that** [benefit].

---

## 1. Room List Page (`/rooms`)

### Viewing Rooms

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| RM-01 | As a player, I want to see a list of active rooms, so that I can find a group to play with. | - Shows all OPEN/APPLY rooms<br>- Real-time updates via WebSocket<br>- Empty state when no rooms |
| RM-02 | As a player, I want to filter rooms by platform, boss, region, and mic preference, so that I can find compatible groups. | - Filter dropdowns persist selection<br>- Results update immediately<br>- "Clear filters" button |
| RM-03 | As a player, I want to see room cards with host info, player count, and mode, so that I can quickly assess rooms. | - Shows: Boss, platform, region, mic, "2/3 ●●○"<br>- Host avatar + name + reputation badge<br>- Room description (truncated) |
| RM-04 | As a player, I want to hover on a username to see a mini profile card, so that I can check their reputation without leaving the page. | - Popup shows: name, tier badge, run count, avg rating<br>- Click navigates to full profile |
| RM-05 | As a player, I want to see how long a room has been open, so that I can avoid stale listings. | - Shows "Created X min ago"<br>- Updates in real-time |
| RM-06 | As a player, I want to filter rooms by game mode (Normal/Deep of Night), so that I can find the right match type. | - Game mode filter toggle<br>- DON rooms show depth badge |
| RM-07 | As a Deep of Night player, I want to filter by depth level (1-5), so that I can find teams at my skill level. | - Depth filter (only visible when DON selected)<br>- Shows "Depth 3" badge on room cards |

### Joining Rooms

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| RM-06 | As a player, I want to click "Join" on an OPEN room to join immediately, so that I can quickly get into a group. | - Adds player to room (PENDING state)<br>- Shows VC link with 5min countdown<br>- Redirects to room view |
| RM-07 | As a player, I want to click "Apply" on an APPLY room to request access, so that the host can review my profile. | - Sends application to host<br>- Shows "Application sent" status<br>- Player can cancel application |
| RM-08 | As a player, I want to see which rooms I've applied to, so that I can track my pending applications. | - "Applied" badge on room card<br>- Can withdraw application |
| RM-09 | As a player, I want to join a LOCKED room by entering the password, so that I can join friends mid-session. | - Password input field<br>- Room password = in-game password<br>- Joins if password matches |

---

## 2. Create Room Page (`/rooms/create`)

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| CR-01 | As a player, I want to create a room with platform, boss, region, mic, and party size settings, so that I can find compatible teammates. | - Form with all fields<br>- Validation on required fields<br>- Party size: 3 or 6 (Seamless only) |
| CR-02 | As a player, I want to choose between OPEN and APPLY room modes, so that I can control who joins. | - Toggle between modes<br>- APPLY shows explanation |
| CR-03 | As a player, I want to add an optional description (300 chars), so that I can specify requirements. | - Character counter<br>- Truncates at limit |
| CR-04 | As a player, I want to create a room and be redirected to the room view, so that I can start hosting. | - Creates room in CREATED state<br>- Creates private VC in Discord<br>- Redirects to `/rooms/{id}` |
| CR-05 | As a player, I want to select Normal or Deep of Night mode when creating a room, so that I can find teammates for ranked play. | - Game mode toggle (Normal/Deep of Night)<br>- Deep of Night shows depth selector |
| CR-06 | As a Deep of Night player, I want to select target depth (1-5), so that I can match with similar-skill players. | - Depth 1-5 dropdown (only for DON)<br>- Boss selector hidden (DON = random boss) |
| CR-07 | As a host, I want to select my Nightfarer when creating a room, so that teammates know what class I'm playing. | - Nightfarer dropdown<br>- Shows on room card |

---

## 3. Room View Page (`/rooms/:id`)

### Room Header

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| RV-01 | As a room member, I want to see room details (boss, platform, region, mode, description), so that I know what I joined. | - Header with all room info<br>- Mode badge (OPEN/APPLY/RUN/LOCKED) |
| RV-02 | As a room member, I want to see the current player list with ready status, so that I know who's in the room. | - List of members with avatars<br>- Ready status: ✅ Ready (in VC) / ⏳ Waiting<br>- Empty slots shown |
| RV-03 | As a room member, I want to see the VC link prominently, so that I can easily join the voice channel. | - Big "Join Voice" button<br>- Shows countdown if in grace period |

### Player Actions

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| RV-04 | As a room member, I want to click "Leave Room", so that I can exit the group. | - Removes from room<br>- Redirects to room list |
| RV-05 | As a player in grace period, I want to see a countdown timer, so that I know how long I have to join VC. | - Shows "Join VC in X:XX or you'll be kicked"<br>- Updates in real-time |

### Host Actions

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| RV-06 | As a host, I want to see pending applicants for APPLY rooms, so that I can accept or reject them. | - List of applicant cards<br>- Shows profile: name, tier, runs, avg rating<br>- Accept/Reject buttons |
| RV-07 | As a host, I want to accept an applicant, so that they can join my room. | - Moves applicant to room (PENDING)<br>- Sends notification to player<br>- Updates room UI |
| RV-08 | As a host, I want to reject an applicant, so that I can decline unsuitable players. | - Removes from applicant list<br>- Sends notification to player |
| RV-09 | As a host, I want to kick a player from the room, so that I can remove problematic members. | - Removes player immediately<br>- Sends notification<br>- Updates room UI |
| RV-10 | As a host, I want to close the room, so that I can end the session. | - Deletes room and VC<br>- Notifies all members<br>- Redirects host to room list |
| RV-11 | As a host, I want to change room mode (OPEN / APPLY / LOCKED), so that I can adjust access control. | - Toggle between modes<br>- LOCKED requires password to join |

### Run Lifecycle

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| RV-12 | As a host, I want to click "Start Run" when everyone is ready, so that we can begin playing. | - Room → RUN mode<br>- Uses host's default password if set, else generates one<br>- Shows password prominently<br>- Sends password to VC chat + DMs |
| RV-13 | As a room member, I want to see the password when run starts, so that I can enter it in-game. | - Large password display<br>- Copy button<br>- Instructions for in-game entry |
| RV-14 | As a host, I want to click "End Run" when we finish, so that the room can accept new players or do another run. | - Room → OPEN/APPLY mode<br>- If run ≥ 10min, prompts reviews<br>- Updates room UI |
| RV-15 | As a host, I want to lock the room manually (without starting a run), so that I can pause matchmaking. | - Room → LOCKED mode<br>- Can unlock later |

---

## 4. Review Page (`/reviews/:runId`)

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| REV-01 | As a player who completed a 10+ min run, I want to receive a review link via Discord DM, so that I can rate my teammates. | - Magic link sent after run ends<br>- Link valid for 24 hours |
| REV-02 | As a player, I want to see my teammates on the review page, so that I can rate each one. | - Shows teammate cards with avatars<br>- One card per teammate |
| REV-03 | As a player, I want to rate each teammate from 1-5 stars, so that I can provide nuanced feedback. | - 5-star rating widget<br>- Required to submit |
| REV-04 | As a player, I want to optionally add a comment to my rating, so that I can provide details. | - Text field (optional)<br>- Max 200 chars |
| REV-05 | As a player, I want to skip rating a teammate, so that I don't have to rate everyone. | - Skip button per teammate<br>- Can submit partial reviews |
| REV-06 | As a player, I want to submit my reviews, so that they're recorded in the system. | - Submit button<br>- Confirmation message<br>- Redirects to dashboard |

---

## 5. Profile Page (`/profile/:id`)

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| PR-01 | As a player, I want to view any user's profile, so that I can check their reputation. | - Public profiles for all users<br>- Shows avatar, name, badges |
| PR-02 | As a player, I want to see a user's stats (runs, avg rating, sherpa count), so that I can assess their experience. | - Stats grid<br>- Average rating from reviews |
| PR-03 | As a player, I want to see a user's reputation tier and badge, so that I can trust their skill level. | - Tier: Newcomer → Reliable → Trusted → Veteran → Legend<br>- Sherpa badges if applicable |
| PR-04 | As a player, I want to see a user's recent runs (last 20), so that I can see their activity. | - List of recent runs<br>- Shows: boss, result (✅/❌), duration, time ago<br>- Can be hidden by user |
| PR-05 | As a player, I want to see a user's preferences (platform, mic, vibe), so that I know their playstyle. | - Shows preferences section |
| PR-06 | As a player, I want to see a user's main Nightfarers, so that I know what classes they play. | - Shows top 1-3 Nightfarers<br>- Based on run history or manual selection |
| PR-07 | As a player, I want to see a user's platform friend links (Steam/PSN/Xbox), so that I can add them in-game. | - Clickable Steam friend code/link<br>- PSN ID (text, no link available)<br>- Xbox Gamertag link |

### My Profile

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| PR-08 | As a logged-in user, I want to edit my preferences, so that I can update my playstyle. | - Edit button on my profile<br>- Form for platform, mic, vibe |
| PR-09 | As a logged-in user, I want to hide my recent runs, so that I can maintain privacy. | - Toggle in settings<br>- Hidden runs not shown to others |
| PR-10 | As a logged-in user, I want to set a default password for my rooms, so that I don't have to remember random passwords. | - Optional field in preferences<br>- Used when starting runs if set<br>- Otherwise system generates one |
| PR-11 | As a logged-in user, I want to set my main Nightfarers, so that others know my preferred classes. | - Multi-select (1-3 Nightfarers)<br>- Displayed on profile |
| PR-12 | As a logged-in user, I want to add my platform friend links, so that teammates can add me in-game. | - Steam: Friend Code or profile URL<br>- PSN: PSN ID text field<br>- Xbox: Gamertag (auto-generates link) |

---

## 6. Dashboard Page (`/`)

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| DB-01 | As a player, I want to see quick action buttons (Create Room, Browse Rooms), so that I can quickly start matchmaking. | - Prominent action cards<br>- Links to respective pages |
| DB-02 | As a player, I want to see my current room (if any) as a sticky banner, so that I can quickly return to it. | - Banner at top of dashboard<br>- Shows room info + "Return" button<br>- Hidden if not in a room |
| DB-03 | As a player, I want to see my stats summary, so that I can track my progress. | - Runs, avg rating, reputation tier |
| DB-04 | As a player, I want to see active room count and online players, so that I know how active the platform is. | - Real-time counters |

---

## 7. Discord Bot Integration

### Room Management

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| BOT-00 | As a Discord user, I want to see a pinned intro embed in #lfg with action buttons, so that I can easily create rooms or browse. | - Pinned embed in #lfg channel<br>- Buttons: [🟣 Create Room] [📋 Browse Rooms] [🆘 Request Help]<br>- Buttons trigger modals or magic links |
| BOT-01 | As a Discord user, I want to use `/createroom` to create a room, so that I don't need to visit the website. | - Modal with room options<br>- Creates room + VC<br>- Posts room card in #lfg |
| BOT-02 | As a Discord user, I want to see room cards in #lfg channel, so that I can browse rooms in Discord. | - Embed with room info<br>- Join/Apply buttons<br>- Updates in real-time |
| BOT-03 | As a Discord user, I want to click "Join" on a room card to join the room, so that I can join quickly from Discord. | - Adds to room<br>- DMs VC link |

### Notifications

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| BOT-04 | As a host, I want to receive a DM when someone applies to my room, so that I can review them quickly. | - DM with applicant info<br>- Link to web room view |
| BOT-05 | As a player, I want to receive a DM when my application is accepted/rejected, so that I know the result. | - DM with result<br>- VC link if accepted |
| BOT-06 | As a room member, I want to receive a DM when the run starts with the password, so that I can enter the game. | - DM with password<br>- Copy button |
| BOT-07 | As a player, I want to receive a review link DM after a valid run, so that I can rate teammates. | - Magic link to web review page |

### Voice Channel

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| BOT-08 | As a player, I want a private VC created for my room, so that only room members can join. | - VC with permission overwrites<br>- Only room members can see/join |
| BOT-09 | As a system, I want to track VC presence to mark players as READY, so that VC = room membership. | - `on_voice_state_update` events<br>- Updates player state in backend |
| BOT-10 | As a system, I want to delete the VC when the room closes, so that channels are cleaned up. | - VC deleted 5min after empty<br>- Or immediately on room close |

---

## 8. Real-Time Updates

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| RT-01 | As a player viewing room list, I want to see new rooms appear in real-time, so that I don't miss opportunities. | - WebSocket push for new rooms<br>- Smooth insertion into list |
| RT-02 | As a player viewing room list, I want to see room updates (player count, mode) in real-time, so that I have accurate info. | - Updates without page refresh |
| RT-03 | As a room member, I want to see player join/leave in real-time, so that I know current room state. | - WebSocket push for member changes |
| RT-04 | As a room member, I want to see mode changes (RUN, LOCKED) in real-time, so that I'm notified immediately. | - UI updates immediately<br>- Password shown when run starts |

---

## 9. Grace Period & Auto-Kick

| ID | Story | Acceptance Criteria |
|----|-------|---------------------|
| GP-01 | As a player who joined a room, I want a 5-minute window to join VC, so that I have time to connect. | - Timer starts on room join<br>- Auto-kicked if timer expires |
| GP-02 | As a player who disconnected from VC, I want a 5-minute grace period to rejoin, so that I'm not kicked for temporary issues. | - Timer starts on VC leave<br>- DM notification with warning |
| GP-03 | As a player in grace period, I want to see a countdown on the room view, so that I know how much time I have. | - Timer visible on UI<br>- Updates in real-time |
| GP-04 | As a host whose room has no activity for 10 minutes, I want the room to be expired automatically, so that stale rooms are cleaned up. | - Room deleted if host not in VC for 10min<br>- Notifications sent |

---

## Priority Matrix

| Priority | Epics |
|----------|-------|
| **P0 - MVP** | Room List, Create Room, Room View (basic), Discord VC integration |
| **P1 - Core** | Host actions, Grace period, Real-time updates |
| **P2 - Enhanced** | Reviews, Profile page, Dashboard |
| **P3 - Polish** | Filters, Mini-cards, Advanced Discord features |
