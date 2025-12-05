# Nightreign Nexus — User Personas

**Document Version:** 1.0  
**Date:** December 2024

---

## Persona 1: The Quick Queuer

**Archetype:** Casual Tarnished  
**Goal:** Play immediately with minimal friction

| Attribute | Detail |
|-----------|--------|
| Playtime | 1-2 hours/session, evenings |
| Skill Level | Mid-tier, knows mechanics |
| Social Preference | Doesn't care who, just wants bodies |
| Mic | Optional, usually off |
| Patience | Low — will leave if wait > 2 min |

**Behavior Pattern:**
```
Open Discord → Type "lfg" → Wait 30 sec → No response → Type again → 
Get frustrated → Join random VC → Hope for the best
```

**Pain Points:**
- P1 (Duplicate Listings) — Has to repost constantly
- P3 (Voice Friction) — Hates the "can you make a VC?" dance
- P8 (No Lifecycle) — Can't tell if old posts are still active

**Success Metric:** Time from app open → in-game < 90 seconds

---

## Persona 2: The War Room Commander

**Archetype:** Abyssal Walker (Hardcore)  
**Goal:** Assemble optimal team for Deep Night / high-difficulty content

| Attribute | Detail |
|-----------|--------|
| Playtime | 3-4 hour sessions, scheduled |
| Skill Level | High, theory-crafts builds |
| Social Preference | Curated team, vetted players |
| Mic | Required |
| Patience | High for setup, zero for bad players |

**Behavior Pattern:**
```
Check who's online → DM known players → If unavailable, reluctantly post LFG →
Vet applicants → Ask about builds → Reject randoms → Finally start after 20 min
```

**Pain Points:**
- P5 (No Skill Matching) — Wastes time with undergeared randoms
- P11 (No Reputation) — Can't verify if applicant is good
- P2 (Ambiguous Slots) — Needs exact party composition control

**Success Metric:** Match quality score (did team clear first try?)

---

## Persona 3: The SOS Sender

**Archetype:** Stuck Player  
**Goal:** Get past a specific boss blocking progression

| Attribute | Detail |
|-----------|--------|
| Playtime | Sporadic, often late night |
| Skill Level | Variable, hit a wall |
| Social Preference | Needs a carry, grateful for help |
| Mic | Willing if needed |
| Patience | Will wait for the right help |

**Behavior Pattern:**
```
Die to boss 10x → Search "how to beat X" → Give up on solo →
Post "need help with X" → Get ignored → Try again → 
Eventually someone responds → Grateful forever
```

**Pain Points:**
- P7 (Ephemeral Threading) — Help offers get buried
- P10 (Time Blindness) — Can't schedule, posts expire
- P6 (Playstyle Signals) — Needs to signal "I'm learning, be patient"

**Success Metric:** Time from SOS → helper arrives < 5 minutes

---

## Persona 4: The Sherpa

**Archetype:** Veteran Helper  
**Goal:** Carry others for fun, reputation, or rewards

| Attribute | Detail |
|-----------|--------|
| Playtime | Flexible, often "on call" |
| Skill Level | Expert, mastered content |
| Social Preference | Enjoys teaching, tolerant of newbies |
| Mic | Usually on, explains mechanics |
| Patience | High for learners, low for toxicity |

**Behavior Pattern:**
```
Finished own content → Bored → Check help channel →
See SOS request → Evaluate if interesting → Join and carry →
Feel good about helping
```

**Pain Points:**
- P11 (No Reputation) — No recognition for helping
- P8 (No Lifecycle) — Can't tell which SOS posts are still active
- P7 (Ephemeral Threading) — Offers to help get lost

**Success Metric:** Visibility of help requests, reputation/karma system

---

## Persona Priority Matrix

| Persona | Population % | Revenue Potential | Design Priority |
|---------|:------------:|:-----------------:|:---------------:|
| Quick Queuer | 60% | Low | 🔴 High |
| War Room Commander | 15% | High (premium) | 🔴 High |
| SOS Sender | 15% | Medium | 🟡 Medium |
| Sherpa | 10% | Medium (retention) | 🟡 Medium |

**Design Implication:** The platform must serve Quick Queuers with speed while providing depth for War Room Commanders. SOS/Sherpa flows are secondary but critical for community health.
