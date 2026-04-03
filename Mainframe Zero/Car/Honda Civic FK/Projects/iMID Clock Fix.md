---
tags: [project, civic]
status: 🔄 In Progress
priority: 2
area: Electronics / iMID
started: 2026-03-01
completed: 
cost_est: 150
cost_actual: 
---

# 🖥️ iMID Clock Fix

## Goal

Fix the iMID clock that resets to 00:00 after every engine stop on the nav-equipped FK model.

## Approach

Swap the current nav iMID unit for a TR0 (non-nav) iMID module which does not have this reset behaviour. The TR0 unit retains clock via a different internal power rail logic.

## Options evaluated

| Option | Verdict | Notes |
|---|---|---|
| Battery disconnect trick | ❌ Ruled out | Doesn't fix root cause |
| PAC RP4-HD11 | ❌ Ruled out | Nav-equipped model incompatible |
| Dealer ECU reflash | ❌ Ruled out | Honda Poland confirmed not fixable |
| iMID TR0 module swap | ✅ Viable | Source via Allegro / Ovoko / scrapyard |

## Parts needed

```dataview
TABLE WITHOUT ID
  file.link AS "Part",
  source AS "Source",
  cost AS "Cost (PLN)",
  status AS "Status"
FROM "Car/Honda Civic FK/Parts"
WHERE project_link = "iMID Clock Fix"
```

## Work log entries

```dataview
TABLE WITHOUT ID
  file.link AS "Entry",
  date AS "Date",
  cost AS "Cost (PLN)"
FROM "Car/Honda Civic FK/Work Log"
WHERE project = "iMID Clock Fix"
SORT date ASC
```

## Research notes

- FK nav iMID part: look for non-nav (TR0) equivalent — check part number on unit label
- Scrapyard search terms: Honda Civic 9th gen iMID, display unit FK, 39100-TR0
- Forum threads confirm TR0 swap works, no coding required

## Next step

- [ ] Source TR0 iMID unit — Allegro / Ovoko / złomowisko
- [ ] Verify part number compatibility before buying

---
*[[Car/Honda Civic FK/Dashboard|← Dashboard]]*
