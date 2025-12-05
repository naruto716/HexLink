# Elden Ring Nightreign — Game Mechanics Report

**Purpose:** Platform-relevant game knowledge for LFG system design  
**Last Updated:** December 2024

---

## Game Overview

**Elden Ring Nightreign** is a roguelike co-op spin-off released May 30, 2025. It's designed for **3-player co-op** with a ~45-60 minute run structure.

---

## Run Structure

```
┌─────────────────────────────────────────────────────────────┐
│ NIGHT 1             NIGHT 2             NIGHT 3             │
│ ┌─────────────┐    ┌─────────────┐    ┌───────────────────┐│
│ │ Explore     │    │ Explore     │    │                   ││
│ │ Loot        │ →  │ Loot        │ →  │    NIGHTLORD      ││
│ │ Level up    │    │ Level up    │    │      BOSS         ││
│ ├─────────────┤    ├─────────────┤    │                   ││
│ │ Night Boss  │    │ Night Boss  │    │                   ││
│ └─────────────┘    └─────────────┘    └───────────────────┘│
│                                                             │
│ ~15-20 min          ~15-20 min         ~10-15 min          │
└─────────────────────────────────────────────────────────────┘
```

### Key Mechanics

| Feature | How it Works |
|---------|--------------|
| **Shrinking Zone** | "Night's Tide" shrinks playable area (like battle royale) |
| **Auto-Leveling** | Invest runes into levels, not individual stats |
| **Shared Loot** | Normal items shared, boss rewards are personal |
| **Revival** | Attack downed teammates to revive them |
| **No Equip Load** | Simplified inventory, no weight management |
| **Enhanced Mobility** | Wall-running, gliding via spectral hawk, no fall damage |

---

## Multiplayer System

### Three Ways to Form a Party

| Method | How It Works | Platform Relevance |
|--------|--------------|-------------------|
| **1. In-Game Invite** | Expedition Table → Invite Members → Select from platform friends list | Requires being friends on Steam/PSN/Xbox |
| **2. Steam Overlay Invite** | Shift+Tab → Friends → Right-click → "Invite to Party" | PC only, instant join |
| **3. Password Matching** | Both players enter same password → Matchmaking connects them | Works without being friends |

### Password Types

| Type | Purpose | Platform Use |
|------|---------|--------------|
| **Multiplayer Password** | Private match — ONLY players with this password can join | Our platform generates this |
| **Group Password** | Soft priority — increased chance to match, but randoms can still join | Could use for server-wide community buff |

### "No. of Password Players" Setting

Critical setting in Matchmaking Settings:

| Setting | Result |
|---------|--------|
| 3 | All 3 slots require password (fully private) |
| 2 | 2 players use password, 3rd is random matchmaking |
| 1 | Only host uses password (essentially random) |

**Platform Implication:** When we have a 2/3 room, users should set "No. of Password Players" = 2, and 3rd can be auto-filled

### Step-by-Step: How Users Join via Password

```
1. Get password from Nexus platform (e.g., "Nx882")
2. Launch Elden Ring Nightreign
3. Go to Roundtable Hold
4. Interact with Expedition Table
5. Press F2 (or RB) → Matchmaking Settings
6. Enter "Nx882" in Multiplayer Password field
7. Set "No. of Password Players" = 3 (for full private)
8. Select target Nightlord
9. Queue → Matches with others using same password
```

### Cross-Platform Notes

| Feature | Status |
|---------|--------|
| Cross-play (PC ↔ Console) | ❌ Not supported |
| Cross-gen (PS4 ↔ PS5) | ✅ Supported |
| Cross-gen (Xbox One ↔ Series X) | ✅ Supported |

**Platform Implication:** Platform filter is REQUIRED — PC players cannot match with PlayStation

### Boss Unlock Requirements

| Boss | Requirement to Matchmake |
|------|-------------------------|
| Gladius (Tricephalos) | None — always available |
| All other Nightlords | Must defeat Tricephalos first in own session |

**Note:** Can still join friends via direct invite even without unlocking

**Platform Implication:** New players can only queue for Gladius until they clear it

---

## Nightlords (Final Bosses)

Players select which Nightlord to fight before starting:

| Nightlord | Expedition Name | Notes |
|-----------|-----------------|-------|
| **Gladius, Beast of Night** | Tricephalos | Three-headed dog, often first boss |
| **Adel, Baron of Night** | Gaping Jaw | - |
| **Gnoster, Wisdom of Night** | Sentient Pest | Two-part: scorpion + butterfly |
| **Maris, Fathom of Night** | Augur | - |
| **Libra, Creature of Night** | Equilibrious Beast | Goat-man, offers deals |
| **Fulghor, Champion of Nightglow** | Darkdrift Knight | - |
| **Caligo, Miasma of Night** | Fissure in the Fog | Frost dragon |
| **Heolstor, the Nightlord** | Night Aspect | True final boss |

**Platform Implication:** Boss selector dropdown should list these 8 Nightlords

---

## Night Bosses (Night 1 & 2)

Random bosses at end of each night, drawn from:
- Elden Ring base game bosses
- Shadow of the Erdtree DLC bosses
- Dark Souls series bosses (remixed)

Examples: Smelter Demon, Dancer of the Boreal Valley, Nameless King, Grafted Monarch

**Platform Implication:** Night boss is random, only Nightlord can be selected

---

## Nightfarers (Playable Characters)

Not classes — fixed heroes with unique abilities:

| Nightfarer | Focus | Role |
|------------|-------|------|
| **Wylder** | STR/DEX hybrid | All-rounder, grappling hook |
| **Guardian** | STR, shields | Tank, ally reviver |
| **Raider** | Melee | DPS |
| **Ironeye** | Bow | Ranged DPS |
| **Revenant** | - | - |
| **Executor** | - | - |
| **Duchess** | - | - |
| **Undertaker** | STR/Faith (DLC) | Tank, kills Nightlord |
| **Scholar** | Arcane (DLC) | Support, observations |

**Platform Implication:** Profile could track "main Nightfarer" or allow filtering

---

## Deep of Night (Ranked Mode)

Endgame mode released September 2025. Requires defeating Heolstor first.

### Depth Rating System

| Depth | Rating | Difficulty |
|:-----:|:------:|------------|
| 1 | 0-999 | Base difficulty |
| 2 | 1000-1999 | Harder |
| 3 | 2000-3999 | Much harder |
| 4 | 4000-5999 | Extreme |
| 5 | 6000+ | Endless battle |

### Deep of Night Features

- Enemies have more HP and damage
- Random Nightlord (can't choose)
- Unique "Depths Relics" with trade-offs
- Matched with same-Depth players

**Platform Implication:**
- Add "Deep of Night" as separate mode
- Show player's Depth rating on profile
- Filter rooms by Depth level

---

## DLC: The Forsaken Hollows (Dec 4, 2025)

### New Content

| Addition | Details |
|----------|---------|
| **New Area** | The Shifting Earth — underground ruins |
| **New Nightfarers** | Undertaker, Scholar |
| **New Boss** | Balancers (Weapon-Bequeathed Harmonia) — mob of angels |
| **Returning Bosses** | Artorias the Abysswalker, Demon Princes |

### Unlocking DLC Content

1. Defeat Tricephalos
2. Speak with Iron Menial in Roundtable Hold
3. Unlock Undertaker + Scholar
4. Defeat 2+ bosses to unlock Balancers expedition

**Platform Implication:**
- Add "Balancers" to boss selector
- Add "Shifting Earth" to area/event filters
- Track if user has DLC unlocked?

---

## Platform Design Implications Summary

### Boss Selector Dropdown
```
Nightlords:
├── Gladius (Tricephalos)
├── Adel (Gaping Jaw)
├── Gnoster (Sentient Pest)
├── Maris (Augur)
├── Libra (Equilibrious Beast)
├── Fulghor (Darkdrift Knight)
├── Caligo (Fissure in the Fog)
├── Heolstor (Night Aspect)
└── Balancers (DLC)

Modes:
├── Normal
├── Deep of Night (show Depth 1-5)
└── Shifting Earth (DLC events)
```

### Room Creation Form Fields

| Field | Values |
|-------|--------|
| Platform | PC, PlayStation, Xbox, Seamless Coop |
| Party Size | 3 (or up to 6 for Seamless) |
| Mode | Normal, Deep of Night |
| Depth (if Deep) | 1, 2, 3, 4, 5 |
| Nightlord | 8 base + Balancers |
| Nightfarer Filter | Optional - looking for specific hero |

### Profile Stats to Track

- Runs completed
- Nightlords defeated (by name)
- Deep of Night Depth rating
- Main Nightfarer(s)
- DLC owned
