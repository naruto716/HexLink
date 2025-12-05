# Reputation System Requirements

## Metrics

| Metric | Points | Trigger |
|--------|:------:|---------|
| Run Completed | +10 | Finish all 3 nights |
| Run Abandoned | -15 | Leave after room locks |
| Sherpa Assist | +20 | Complete SOS as helper |
| Thumbs Up | +5 | Teammate upvote |
| Thumbs Down | -5 | Teammate downvote |

---

## Tiers

| Score | Tier | Badge |
|------:|------|:-----:|
| 0-49 | Newcomer | ⚪ |
| 50-149 | Reliable | 🟢 |
| 150-299 | Trusted | 🔵 |
| 300-499 | Veteran | 🟣 |
| 500+ | Legend | 🟡 |

### Sherpa Badges (Additional)

| Assists | Badge |
|--------:|-------|
| 10+ | 🛡️ Helper |
| 50+ | 🛡️🛡️ Guardian |
| 100+ | 🛡️🛡️🛡️ Legendary |

---

## Post-Run Rating

**REQ-REP-01:** After run ends, prompt all players to rate teammates  
**REQ-REP-02:** Options: 👍 / 👎 / Skip  
**REQ-REP-03:** Ratings are anonymous  
**REQ-REP-04:** Prompt via Discord DM + Web modal

---

## Abandonment

**REQ-REP-05:** Leave after Lock = Abandon penalty  
**REQ-REP-06:** Disconnect = 5 min grace period to rejoin  
**REQ-REP-07:** Rejoin within grace = No penalty

---

## Anti-Abuse

**REQ-REP-08:** Max 1 thumbs up per unique player per 24h  
**REQ-REP-09:** Can only rate players from shared runs  
**REQ-REP-10:** New accounts (<7 days) capped at Reliable tier

---

## Display

**REQ-REP-11:** Badge shown next to username everywhere  
**REQ-REP-12:** Room list shows host tier + run count  
**REQ-REP-13:** Applicant cards show completion rate + sherpa count  
**REQ-REP-14:** Optional filter: "Show only Trusted+ hosts"
