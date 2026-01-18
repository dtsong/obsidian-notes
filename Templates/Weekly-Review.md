---
week: <% tp.date.now("YYYY-[W]ww") %>
start_date: <% tp.date.weekday("YYYY-MM-DD", 1) %>
end_date: <% tp.date.weekday("YYYY-MM-DD", 5) %>
tags: review, weekly
---

# Week <% tp.date.now("ww") %> Review (<% tp.date.weekday("MMM D", 1) %> - <% tp.date.weekday("MMM D", 5) %>)

## 🎯 Week Summary
> 2-3 sentence overview of how the week went

## 📊 This Week's Daily Notes
```dataview
TABLE WITHOUT ID
  link(file.name) as "Day",
  file.frontmatter.tags as "Energy"
FROM "01-Daily"
WHERE date >= date(<% tp.date.weekday("YYYY-MM-DD", 1) %>) AND date <= date(<% tp.date.weekday("YYYY-MM-DD", 7) %>)
SORT date ASC
```

## 🏆 Wins This Week
```dataview
LIST
FROM "02-Career/Brag-Doc"
WHERE date >= date(<% tp.date.weekday("YYYY-MM-DD", 1) %>) AND date <= date(<% tp.date.weekday("YYYY-MM-DD", 7) %>)
SORT date DESC
```

### Manual additions
- 

## 📈 Progress on Goals

### Technical Growth
| Goal | Status | Notes |
|------|--------|-------|
| | | |

### Projects
| Project | Progress | Next Steps |
|---------|----------|------------|
| | | |

## 🌱 What I Learned
- 

## 🔄 Carry Forward to Next Week
> Unfinished items or ongoing priorities

- [ ] 

## 💭 Reflection

**What went well this week?**


**What didn't go as planned?**


**What will I do differently next week?**


**Energy/mood trend:** 


---

## 📝 Notes for Performance Review
> Anything from this week worth highlighting later?

