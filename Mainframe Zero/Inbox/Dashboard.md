# Inbox

## New
```dataview
TABLE WITHOUT ID
  file.link AS "Note",
  join(filter(file.tags, (t) => t != "inbox"), " ") AS "Category",
  date AS "Added",
  why-interesting AS "Why"
FROM "Inbox"
WHERE status = "new" AND file.name != "_template" AND file.name != "Dashboard"
SORT date DESC
```

## In Progress
```dataview
TABLE WITHOUT ID
  file.link AS "Note",
  join(filter(file.tags, (t) => t != "inbox"), " ") AS "Category",
  date AS "Added"
FROM "Inbox"
WHERE status = "reading" AND file.name != "_template" AND file.name != "Dashboard"
SORT date DESC
```

## Done
```dataview
TABLE WITHOUT ID
  file.link AS "Note",
  join(filter(file.tags, (t) => t != "inbox"), " ") AS "Category",
  date AS "Added"
FROM "Inbox"
WHERE status = "done" AND file.name != "_template" AND file.name != "Dashboard"
SORT date DESC
```
