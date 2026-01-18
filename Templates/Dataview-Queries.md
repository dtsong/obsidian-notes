# Dataview Queries Cheatsheet

Copy these into any note to create live, auto-updating views of your vault.

---

## Brag Doc Queries

### All Wins This Quarter
```dataview
TABLE category, date, quarter
FROM "02-Career/Brag-Doc"
WHERE quarter = "2025-Q1"
SORT date DESC
```

### Wins by Category
```dataview
TABLE WITHOUT ID
  category as "Category",
  length(rows) as "Count"
FROM "02-Career/Brag-Doc"
GROUP BY category
SORT length(rows) DESC
```

### Recent Wins (Last 30 Days)
```dataview
LIST
FROM "02-Career/Brag-Doc"
WHERE date >= date(today) - dur(30 days)
SORT date DESC
```

---

## Project Queries

### Active Projects
```dataview
TABLE status, priority, updated
FROM "03-Projects"
WHERE status = "active"
SORT priority ASC
```

### All Projects by Status
```dataview
TABLE WITHOUT ID
  link(file.name) as "Project",
  status,
  priority,
  updated
FROM "03-Projects/AI-Projects"
SORT status ASC, priority ASC
```

### Projects Updated This Week
```dataview
LIST
FROM "03-Projects"
WHERE updated >= date(today) - dur(7 days)
SORT updated DESC
```

### Claude Code Generated Notes
```dataview
TABLE date, file.folder
FROM ""
WHERE source = "claude-code"
SORT date DESC
```

---

## Daily Notes Queries

### This Week's Daily Notes
```dataview
TABLE WITHOUT ID
  link(file.name) as "Day",
  length(file.tasks.completed) as "Tasks Done"
FROM "01-Daily"
WHERE date >= date(today) - dur(7 days)
SORT date DESC
```

### Days with Wins Logged
```dataview
LIST
FROM "01-Daily"
WHERE contains(file.content, "## 🏆 Wins") AND !contains(file.content, "## 🏆 Wins
- 

##")
SORT date DESC
LIMIT 10
```

---

## Task Queries

### All Open Tasks
```dataview
TASK
FROM "02-Career" OR "03-Projects"
WHERE !completed
SORT file.name ASC
```

### Tasks Due This Week
```dataview
TASK
WHERE !completed AND due <= date(today) + dur(7 days)
SORT due ASC
```

### Completed Tasks (Last 7 Days)
```dataview
TASK
WHERE completed AND completion >= date(today) - dur(7 days)
SORT completion DESC
```

---

## Growth & Learning Queries

### Active Learning Roadmaps
```dataview
TABLE current_level, target_level, target_date, status
FROM "02-Career/Growth-Roadmap"
WHERE status = "in-progress"
SORT priority ASC
```

### Skills by Priority
```dataview
TABLE WITHOUT ID
  link(file.name) as "Skill",
  current_level as "Current",
  target_level as "Target",
  status
FROM "02-Career/Growth-Roadmap"
SORT priority ASC
```

---

## Pokemon TCG Queries

### Decks by Status
```dataview
TABLE archetype, status, win_rate
FROM "04-Pokemon-TCG/Decks"
SORT status ASC
```

### Upcoming Events
```dataview
TABLE event_date, event_type, format, status
FROM "04-Pokemon-TCG/Events"
WHERE status != "completed" AND status != "cancelled"
SORT event_date ASC
```

### Event Results
```dataview
TABLE event_date, event_type, location
FROM "04-Pokemon-TCG/Events"
WHERE status = "completed"
SORT event_date DESC
```

---

## Dashboard Queries (for Homepage)

### Quick Stats
```dataview
TABLE WITHOUT ID
  "📝 Daily Notes" as "Category",
  length(filter(file.lists, (x) => x.text != "")) as "Count"
FROM "01-Daily"
WHERE date >= date(today) - dur(30 days)
```

### Recently Modified
```dataview
TABLE WITHOUT ID
  link(file.name) as "Note",
  file.folder as "Folder",
  file.mtime as "Modified"
FROM ""
WHERE file.name != "Dataview-Queries"
SORT file.mtime DESC
LIMIT 10
```

### Orphan Notes (no links in or out)
```dataview
LIST
FROM ""
WHERE length(file.inlinks) = 0 AND length(file.outlinks) = 0
LIMIT 10
```

---

## Tips

1. **Test queries** in a scratch note before adding to templates
2. **Performance:** Avoid `FROM ""` (searches entire vault) when possible
3. **Debugging:** If a query returns nothing, check:
   - Frontmatter field names match exactly (case-sensitive)
   - Folder paths are correct
   - Date formats match what you're filtering

## Dataview Field Reference

Common frontmatter fields used in these templates:
- `date` - Note creation date
- `updated` - Last modified
- `status` - Workflow state
- `priority` - High/medium/low
- `tags` - Array of tags
- `category` - For brag entries
- `quarter` - e.g., "2025-Q1"
- `source` - "human" or "claude-code"
