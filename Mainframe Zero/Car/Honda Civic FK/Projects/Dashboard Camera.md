---
tags:
  - project
  - civic
status: 🔍 Researching
priority: 2
area:
started:
completed:
cost_est:
cost_actual:
---

# Dashboard Camera
## Goal

_One sentence: what does success look like?_

## Approach

_How are you tackling this?_

## Parts needed

```dataview
TABLE WITHOUT ID
  file.link AS "Part",
  source AS "Source",
  cost AS "Cost (PLN)",
  status AS "Status"
FROM "Car/Honda Civic FK/Parts"
WHERE project_link = this.file.name
```

## Work log entries

```dataview
TABLE WITHOUT ID
  file.link AS "Entry",
  date AS "Date",
  cost AS "Cost (PLN)"
FROM "Car/Honda Civic FK/Work Log"
WHERE project = this.file.name
SORT date ASC
```

## Research notes

_Links, forum posts, part numbers, anything gathered during research._

## Next step

- [ ] _Fill in_

---
*[[Car/Honda Civic FK/Dashboard|← Dashboard]]*
