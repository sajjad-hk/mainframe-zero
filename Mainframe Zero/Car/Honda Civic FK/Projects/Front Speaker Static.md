---
tags: [project, civic]
status: 🔍 Diagnosing
priority: 3
area: Audio
started: 2026-03-01
completed: 
cost_est: 80
cost_actual: 
---

# 🔊 Front Speaker Static

## Goal

Eliminate static / noise from front speakers when using the Joying Android 12 head unit.

## Root cause hypothesis

Factory amp in the FK is being driven by the Joying's line output at an incompatible voltage level, causing noise floor issues or ground loop interference.

## Diagnostic steps

- [ ] Step 1 — Reduce Joying output gain in settings → retest
- [ ] Step 2 — If noise persists: install ground loop isolator on front speaker lines → retest
- [ ] Step 3 — If still present: consider bypassing factory amp entirely

## Parts needed

```dataview
TABLE WITHOUT ID
  file.link AS "Part",
  source AS "Source",
  cost AS "Cost (PLN)",
  status AS "Status"
FROM "Car/Honda Civic FK/Parts"
WHERE project_link = "Front Speaker Static"
```

## Work log entries

```dataview
TABLE WITHOUT ID
  file.link AS "Entry",
  date AS "Date",
  cost AS "Cost (PLN)"
FROM "Car/Honda Civic FK/Work Log"
WHERE project = "Front Speaker Static"
SORT date ASC
```

## Research notes

- Honda Civic FK 9th gen has factory amp integrated in door cards
- Joying line output default may be too high for factory amp sensitivity
- Ground loop isolators (e.g. PAC SNI-1) cost ~50 PLN, easy install

## Next step

- [ ] Test Joying gain reduction first — zero cost, do this first

---
*[[Car/Honda Civic FK/Dashboard|← Dashboard]]*
