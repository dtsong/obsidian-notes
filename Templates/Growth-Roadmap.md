---
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
tags: growth, roadmap
skill_area: <% tp.file.cursor() %>
target_level: <% await tp.system.suggester(["beginner", "intermediate", "advanced", "expert"], ["beginner", "intermediate", "advanced", "expert"]) %>
current_level: <% await tp.system.suggester(["none", "beginner", "intermediate", "advanced"], ["none", "beginner", "intermediate", "advanced"]) %>
priority: <% await tp.system.suggester(["high", "medium", "low"], ["high", "medium", "low"]) %>
status: <% await tp.system.suggester(["not-started", "in-progress", "paused", "completed"], ["not-started", "in-progress", "paused", "completed"]) %>
target_date: 
---

# 

## Why This Skill?
> Why is learning this important for your career/goals?

## Current State
**What I already know:**
- 

**Gaps to fill:**
- 

## Learning Path

### Phase 1: Foundations
- [ ] 
- [ ] 
**Resources:**
- 

### Phase 2: Intermediate
- [ ] 
- [ ] 
**Resources:**
- 

### Phase 3: Advanced/Applied
- [ ] 
- [ ] 
**Resources:**
- 

## Milestones
| Milestone | Target Date | Completed | Evidence |
|-----------|-------------|-----------|----------|
| | | | |

## Practice Projects
| Project | Status | What I Learned |
|---------|--------|----------------|
| | | |

## Progress Log

### <% tp.date.now("YYYY-MM-DD") %>
- 

## Resources Collection
### Courses
- 

### Books
- 

### Articles/Docs
- 

### People to Learn From
- 

## How to Demonstrate This Skill
> For performance reviews, interviews, or portfolio

- 

## Related Brag Doc Entries
```dataview
LIST
FROM "02-Career/Brag-Doc"
WHERE contains(tags, "technical") OR contains(file.content, this.skill_area)
SORT date DESC
LIMIT 5
```

---

## Retrospective (fill when target reached)
**Time invested:**
**Most valuable resource:**
**What I'd do differently:**
**Next skill to develop:** [[02-Career/Growth-Roadmap/]]
