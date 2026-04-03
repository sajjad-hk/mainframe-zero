# 🚗 Honda Civic FK (2012) — Dashboard

> **Model:** Honda Civic 9th Gen (FK) · 2012 · [[Head unit — Joying Android 12]]

| Type                     |            |
| ------------------------ | ---------- |
| Current mileage          | 120*** km  |
| Inspection due           | 2026-11-14 |
| Insurance due            | 2026-11-04 |
| Next oil change          | 128000 km  |
| Corrosion inspection due | 2027-02-28 |

---

## 💰 Cost summary

```dataview
TABLE WITHOUT ID
  sum(rows.cost) AS "Total spent (PLN)"
FROM "Car/Honda Civic FK/Work Log"
WHERE cost != null
```

```dataview
TABLE WITHOUT ID
  category AS "Category",
  sum(rows.cost) AS "Spent (PLN)"
FROM "Car/Honda Civic FK/Work Log"
WHERE cost != null
GROUP BY category
SORT sum(rows.cost) DESC
```

---

## 🔧 Active projects

```dataview
TABLE WITHOUT ID
  file.link AS "Project",
  status AS "Status",
  area AS "Area",
  cost_est AS "Est. (PLN)",
  cost_actual AS "Actual (PLN)"
FROM "Car/Honda Civic FK/Projects"
WHERE status != "✅ Done" AND status != "❌ Abandoned"
SORT priority ASC
```

---

## 📦 Parts — pending

```dataview
TABLE WITHOUT ID
  file.link AS "Part",
  source AS "Source",
  cost AS "Cost (PLN)",
  status AS "Status",
  project_link AS "Project"
FROM "Car/Honda Civic FK/Parts"
WHERE status != "✅ Arrived" AND status != "❌ Cancelled"
SORT file.ctime DESC
```

---

## ✅ Work log — recent

```dataview
TABLE WITHOUT ID
  file.link AS "Entry",
  date AS "Date",
  mileage AS "km",
  category AS "Category",
  cost AS "Cost (PLN)",
  done_by AS "By"
FROM "Car/Honda Civic FK/Work Log"
SORT date DESC
LIMIT 15
```

---

## 📋 All projects

```dataview
TABLE WITHOUT ID
  file.link AS "Project",
  status AS "Status",
  area AS "Area",
  started AS "Started",
  completed AS "Done",
  cost_actual AS "Cost (PLN)"
FROM "Car/Honda Civic FK/Projects"
SORT status ASC, file.ctime DESC
```

---

## 🔗 Quick links

- [[Car/Honda Civic FK/Projects/iMID Clock Fix]]
- [[Car/Honda Civic FK/Projects/Backup Camera Restoration]]
- [[Car/Honda Civic FK/Projects/Front Speaker Static]]
- [[Car/Honda Civic FK/Projects/Rust Protection]]
