---
tags: [project, civic]
status: 🔄 In Progress
priority: 1
area: Underbody / chassis
started: 2026-04-01
completed: 
cost_est: 
cost_actual: 
---

# 🛡️ Rust Protection

## Goal

Treat and seal the underbody and wheel arches to prevent further corrosion on the 2012 FK chassis.

## Approach

DIY underbody treatment: pressure wash, wire brush active rust, apply rust converter where needed, then seal with underbody coating or cavity wax injection for enclosed sections.

## Parts needed

```dataview
TABLE WITHOUT ID
  file.link AS "Part",
  source AS "Source",
  cost AS "Cost (PLN)",
  status AS "Status"
FROM "Car/Honda Civic FK/Parts"
WHERE project_link = "Rust Protection"
```

## Work log entries

```dataview
TABLE WITHOUT ID
  file.link AS "Entry",
  date AS "Date",
  cost AS "Cost (PLN)"
FROM "Car/Honda Civic FK/Work Log"
WHERE project = "Rust Protection"
SORT date ASC
```

## Research notes

- Honda 9th gen FK known rust areas: rear wheel arch seams, sill ends, spare wheel well
- Cavity wax (e.g. Dinitrol, Tectyl) good for hollow sections
- Bitumen-based coating good for flat floor pan areas
- Remove plastic undertray panels before treatment

## Next step

- [ ] Gather materials before session
- [ ] Pressure wash and inspect
- [ ] Document rust spots found with photos

---
*[[Car/Honda Civic FK/Dashboard|← Dashboard]]*
