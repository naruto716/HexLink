# User Profile Requirements

## Profile Data

### Identity (from Discord)
- Discord ID
- Username
- Avatar
- Server roles

### Stats (Platform-tracked)
- Runs completed
- Runs abandoned
- Completion rate (%)
- Sherpa assists
- Thumbs up received
- Thumbs down received
- Reputation score
- Reputation tier

### Preferences
- Preferred platform (PC/PS/Xbox/Seamless)
- Mic preference
- Vibe preference (Chill/Tryhard/Learning)

### History
- Recent runs (last 20)
- Most played bosses
- Frequent teammates

---

## Profile Views

### Mini Card (in room list, applicants)

```
┌──────────────────────────────────────┐
│ WarriorKing 🟣 Veteran               │
│ 142 runs | 94% completion            │
└──────────────────────────────────────┘
```

### Applicant Card (for hosts reviewing)

```
┌──────────────────────────────────────┐
│ Player_A                             │
│ 🔵 Trusted | 28 runs | 96%          │
│ Sherpa: 12 | 👍 89%                 │
│ [✓ Accept]  [✗ Decline]             │
└──────────────────────────────────────┘
```

### Full Profile (web page)

```
┌────────────────────────────────────────────────────────────┐
│ ┌────────┐                                                 │
│ │ Avatar │  WarriorKing                                   │
│ └────────┘  🟣 Veteran 🛡️🛡️ Guardian                     │
│                                                            │
│ STATS                                                      │
│ ┌──────────────┬──────────────┬──────────────┐            │
│ │ 142 Runs     │ 94% Complete │ 28 Sherpa    │            │
│ └──────────────┴──────────────┴──────────────┘            │
│                                                            │
│ PREFERENCES                                                │
│ Platform: PC | Mic: Yes | Vibe: Tryhard                   │
│                                                            │
│ TOP BOSSES                                                 │
│ Gladius (32) • Messmer (28) • Heolstor (21)              │
│                                                            │
│ RECENT RUNS                                                │
│ • Gladius ✅ 47min - 2 hours ago                          │
│ • Messmer ✅ 52min - Yesterday                            │
│ • Adel ❌ Abandoned - 3 days ago                          │
└────────────────────────────────────────────────────────────┘
```

---

## Requirements

**REQ-PROF-01:** Profile syncs Discord identity on each login  
**REQ-PROF-02:** Stats update in real-time after runs  
**REQ-PROF-03:** Profile page accessible via `/profile/:discordId`  
**REQ-PROF-04:** Mini card shown on hover in room list  
**REQ-PROF-05:** Preferences editable on profile page  
**REQ-PROF-06:** Run history shows last 20 runs  
**REQ-PROF-07:** Profile is public (anyone can view)

---

## Privacy

**REQ-PROF-08:** Thumbs down count hidden (only affects score)  
**REQ-PROF-09:** Abandon count hidden (shown as completion %)  
**REQ-PROF-10:** Recent runs can be hidden by user (opt-out)
