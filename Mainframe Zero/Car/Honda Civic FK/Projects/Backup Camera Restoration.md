---
tags: [project, civic]
status: 🔄 In Progress
priority: 2
area: Electronics / Camera
started: 2026-03-01
completed: 
cost_est: 80
cost_actual: 
---

# 📷 Backup Camera Restoration

## Goal

Restore the OEM backup camera feed to the Joying Android 12 head unit via the iMID 32-pin connector.

## Approach

Tap the 32-pin iMID connector for video (Pin 19), ground (Pin 3), and reverse trigger signal (Pin 21). Route to Joying camera input.

## Pin mapping

| Pin | Signal | Notes |
|---|---|---|
| 19 | Video in (composite) | Camera feed |
| 3 | GND | Ground |
| 21 | Reverse trigger | +12V when R gear engaged |

## Parts needed

```dataview
TABLE WITHOUT ID
  file.link AS "Part",
  source AS "Source",
  cost AS "Cost (PLN)",
  status AS "Status"
FROM "Car/Honda Civic FK/Parts"
WHERE project_link = "Backup Camera Restoration"
```

## Work log entries

```dataview
TABLE WITHOUT ID
  file.link AS "Entry",
  date AS "Date",
  cost AS "Cost (PLN)"
FROM "Car/Honda Civic FK/Work Log"
WHERE project = "Backup Camera Restoration"
SORT date ASC
```

## Research notes

- InCarTec 29-735 harness may simplify 32-pin connector access
- Verify composite vs CVBS signal level compatibility with Joying input
- Glovebox area gives access to connector routing

## Next step

- [ ] Physical wiring — tap pins 19, 3, 21
- [ ] Test camera feed in Joying settings
- [ ] Confirm reverse trigger activates automatically

---
*[[Car/Honda Civic FK/Dashboard|← Dashboard]]*
