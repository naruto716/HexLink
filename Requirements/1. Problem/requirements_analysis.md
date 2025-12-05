# Nightreign Nexus — Requirements Analysis

**Document Version:** 1.0  
**Date:** December 2024  
**Analysis Scope:** Discord LFG chat patterns (75 messages, ~3 hours peak activity)

---

## Executive Summary

This document identifies core UX problems in the current Discord-based LFG (Looking For Group) workflow for Elden Ring: Nightreign. Analysis is based on observed user behavior patterns in a 170k-member Discord server.

**Key Finding:** The primary friction isn't Discord itself—it's the lack of structure around party formation, status tracking, and game connection.

---

## Identified Problems

### P1. Duplicate & Stale Listings

Users repost repeatedly because there's no visibility on whether their request was seen or is still active.

| User | Posts | Duration |
|------|-------|----------|
| milk | 3 reposts | 91 minutes |
| The_Lost_One | 3 reposts | 46 minutes |
| JovanJe | 4 reposts | 56 minutes |

**Root Cause:** No feedback loop. Users can't tell if their post is working.

---

### P2. Ambiguous Slot Status

Current shorthand provides incomplete information:

| Message | Ambiguity |
|---------|-----------|
| `lf1` | 1 slot open, but out of how many? (2/3? 3/4?) |
| `lf2` | Need 2, but is the host alone or with a partner? |
| `+2 balancers` | Have 2 or need 2? |

**Root Cause:** No standard format for party composition.

---

### P3. Voice Channel Coordination Friction

Getting into voice requires multi-step coordination:

```
Maki:  lfg balancers, 1st time
san:   i'm up
Maki:  can you create the vc?
san:   yup
san:   https://discord.gg/XJUjfmsmX
san:   @Maki
```

**Observed patterns:**
- 23 messages contained external Discord invite links
- 18 messages referenced existing voice channels
- 34 messages had no voice channel information at all

**Root Cause:** No automated VC creation tied to party formation.

---

### P4. Boss Name Fragmentation

Same content, inconsistent naming:

| Intended Target | Variations Observed |
|-----------------|---------------------|
| Balancers | `balancers`, `Balancers`, `balancer`, `balencers` |
| Dreglord | `dreglord`, `Dreglord`, `Dredge` |
| Tricephalos | `Tricephalos Everdark`, `Everdark Tricephalos` |
| DLC Content | `new boss`, `new dlc boss`, `dlc`, `new dlc` |

**Root Cause:** Freeform text input. No standardized boss selector.

---

### P5. No Experience/Skill Matching

Users express skill expectations, but there's no structured way to filter:

| Message | Implied Need |
|---------|--------------|
| `Lfg with brain for balancer/be a good player` | Skilled players only |
| `i have only 12hrs and i play revenant` | New player seeking patience |
| `chill and learning the boss` | Beginner-friendly |
| `no stressed Players` | Casual atmosphere |

**Root Cause:** No skill/experience indicators on listings or profiles.

---

### P6. Missing Playstyle Signals

Beyond skill, players have communication and vibe preferences:

| Signal | User Quote |
|--------|------------|
| No mic | `no mic/be a good player` |
| Chill | `chill n learn`, `for some chill runs` |
| Frustrated | `beware of spectators and shit team` |
| Tryhard | (implied by skill requirements) |

**Root Cause:** No structured fields for mic requirements or session vibe.

---

### P7. Ephemeral Threading

Messages lack explicit context linking:

```
Nyalos:  whos up for dreglord?
JOIZURA: https://discord.gg/qdBk2VWN
```

Is JOIZURA responding to Nyalos, or posting independently? The 1-minute gap makes it ambiguous.

```
Maki:           lfg balancers, 1st time
san:            i'm up
StatickMaverick: me too
```

Three users want to play—but who joined whose party?

**Root Cause:** Linear chat has no threading or explicit party formation confirmation.

---

### P8. No Request Lifecycle Management

Requests have no state. They're either in chat or buried.

- No way to mark a request as **filled**
- No way to **cancel** a request
- No automatic **expiration**
- Other users can't tell if a post from 30 minutes ago is still active

**Root Cause:** Messages are static text, not stateful objects.

---

### P9. Hidden Password Exchange

Elden Ring multiplayer requires a shared password to connect. In 75 messages:

- **Zero** public password mentions
- Password sharing happens in DMs or voice (invisible)
- High potential for typos and miscommunication

**Root Cause:** No integrated password generation or display.

---

### P10. Time Zone & Availability Blindness

All activity occurs without availability context:

- `anybody up for dlc i am just about to start` — Available now
- `haven't played since deep of night drop` — Returning player, availability unknown

**Root Cause:** No way to indicate "available now" vs "looking for later."

---

### P11. No Trust/Reputation Signals

Users make decisions with zero information about:

- Host reliability (do they ghost?)
- Player skill verification
- Completion rate
- Community standing

**Root Cause:** No profile or reputation system.

---

## Problem Severity Matrix

| # | Problem | Frequency | User Friction | Match Quality |
|---|---------|:---------:|:-------------:|:-------------:|
| P1 | Duplicate Listings | 🔴 | 🔴 | 🟡 |
| P2 | Ambiguous Slots | 🔴 | 🟡 | 🔴 |
| P3 | Voice Channel Friction | 🔴 | 🔴 | 🟡 |
| P4 | Boss Name Fragmentation | 🔴 | 🟡 | 🔴 |
| P5 | No Skill Matching | 🟡 | 🟡 | 🔴 |
| P6 | Missing Playstyle Signals | 🟡 | 🟡 | 🔴 |
| P7 | Ephemeral Threading | 🔴 | 🔴 | 🔴 |
| P8 | No Request Lifecycle | 🔴 | 🟡 | 🟡 |
| P9 | Hidden Password Exchange | 🟢 | 🔴 | 🟡 |
| P10 | Time Blindness | 🟢 | 🟡 | 🟡 |
| P11 | No Reputation | 🟢 | 🟡 | 🔴 |

🔴 High  🟡 Medium  🟢 Low

---

## Next Phase

Solution design addressing each identified problem.
