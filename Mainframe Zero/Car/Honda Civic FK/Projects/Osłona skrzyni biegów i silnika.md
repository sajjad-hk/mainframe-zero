---
tags:
  - project
  - civic
status: 🔄 In Progress
priority: 2
area:
started:
completed:
cost_est:
cost_actual:
---

# Steel Gard to protect bottom of car 
## Goal

_Under the car protection gard to keep engine and gearbox safe_

## Approach

_I found par from internet ordered by phone waiting for production line, then I will go to workshop for installation_

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
