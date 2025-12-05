# Discord Integration Requirements

## Platform Responsibility

| Feature | Web | Discord | Notes |
|---------|:---:|:-------:|-------|
| Browse Rooms | ✅ | - | Better filtering/visual UI |
| Create Room | ✅ | ✅ | `/createroom` command |
| Quick Match | ✅ | ✅ | `/quickmatch` command |
| SOS Request | ✅ | ✅ | `/sos` command |
| Bounty Board | ✅ | - | Needs visual list |
| Notifications | - | ✅ | DMs for match/accept/ready |
| Voice Channel | - | ✅ | Discord only |
| Ready Check | ✅ | ✅ | Buttons in both |

---

## Authentication: Magic Link

**REQ-AUTH-01:** Bot-generated links must contain single-use tokens  
**REQ-AUTH-02:** Tokens expire in 5 minutes  
**REQ-AUTH-03:** Tokens are invalidated after first use  
**REQ-AUTH-04:** Tokens are tied to Discord User ID  
**REQ-AUTH-05:** Fallback to OAuth for direct web visits

```
Discord DM contains:
nexus.gg/room/abc?token=xyz123
         ↓
Token validates → Auto-login → Session created
```

---

## Bot Commands

### /quickmatch

**REQ-CMD-01:** Slash command with options:
- Platform: PC, PlayStation, Xbox, Seamless
- Nightlord: Dropdown (optional, default "Any")

**REQ-CMD-02:** On submit:
- Find existing Open Room OR create new
- DM user with password + VC link

### /createroom

**REQ-CMD-03:** Slash command with options:
- Platform: Required
- Nightlord: Required
- Type: Open / Closed
- Party Size: 3 (or up to 6 for Seamless)
- Mic: Required / Optional
- Vibe: Chill / Tryhard / Learning

**REQ-CMD-04:** On submit:
- Create room in system
- Post to #lfg channel
- Return confirmation with room link

### /sos

**REQ-CMD-05:** Slash command with options:
- Boss: Required
- Note: Text input
- Mic: Yes / No

**REQ-CMD-06:** On submit:
- Post to Bounty Board (web)
- Post embed to #help-requests channel
- Wait for Sherpa response

### /browse

**REQ-CMD-07:** Returns magic link to web room list

---

## Notifications (DMs)

### Match Found

**REQ-NOTIF-01:** DM all matched players with:
- Target boss
- Password (large, copyable)
- Team list
- [Join Voice] button
- [View on Web] magic link

### Application Accepted

**REQ-NOTIF-02:** DM accepted player with:
- Room details
- [Join Voice] button
- [Open Room] magic link
- Note: Other pending applications auto-cancelled

### SOS Sherpa Response

**REQ-NOTIF-03:** DM Caller with:
- Sherpa name
- "Room created, waiting for 3rd"
- [Join Voice] button
- [View Room] magic link

### Ready Check

**REQ-NOTIF-04:** DM all players when room full:
- Current ready status for each player
- [Ready] button
- [Leave] button
- Updates in real-time

### Room Locked

**REQ-NOTIF-05:** DM all players with final:
- Password (prominent)
- [Join Voice] button
- Team list

---

## Channel Posts

### #lfg Channel

**REQ-CHAN-01:** Bot posts Open Rooms as embeds:
```
🔓 OPEN ROOM                              2/3
Target: Gladius
Platform: PC | Chill | No Mic
Host: WarriorKing

[Join Room]
```

**REQ-CHAN-02:** Embed updates when:
- Slot count changes
- Room fills (mark as full)
- Room closes (delete or mark closed)

**REQ-CHAN-03:** Limit to last 20 active rooms (avoid spam)

### #help-requests Channel

**REQ-CHAN-04:** Bot posts SOS requests:
```
🆘 SOS REQUEST
Target: Messmer
"Can't get past phase 2"
Caller: StuckPlayer

[Assist 🛡️]
```

**REQ-CHAN-05:** Delete embed when Sherpa responds

---

## Voice Channels

**REQ-VC-01:** Create temp VC when room locks  
**REQ-VC-02:** VC name format: `Lobby-{password}` (e.g., `Lobby-Nx882`)  
**REQ-VC-03:** Auto-delete VC after 5 min empty  
**REQ-VC-04:** Move matched players to VC (if they grant permission)  
**REQ-VC-05:** Generate instant-join link for DM button

---

## Bidirectional Sync

**REQ-SYNC-01:** Room created on Web → Post to #lfg  
**REQ-SYNC-02:** Room created via /createroom → Visible on Web  
**REQ-SYNC-03:** Join on Web → Discord DM notification  
**REQ-SYNC-04:** Join via Discord button → Web updates slot count  
**REQ-SYNC-05:** Ready on Web → Discord embed updates  
**REQ-SYNC-06:** Ready via Discord button → Web shows ready

---

## Bot Permissions Required

```
SEND_MESSAGES
EMBED_LINKS
USE_SLASH_COMMANDS
MANAGE_CHANNELS (for temp VCs)
MOVE_MEMBERS (optional, for auto-move to VC)
```

---

## Error States

**REQ-ERR-01:** Token expired → Redirect to OAuth  
**REQ-ERR-02:** Room full when joining → "Room is full" message  
**REQ-ERR-03:** Room closed → "Room no longer exists"  
**REQ-ERR-04:** VC creation fails → Provide manual join instructions
