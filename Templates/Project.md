---
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
tags: project, ai
status: <% await tp.system.suggester(["planning", "active", "paused", "completed", "archived"], ["planning", "active", "paused", "completed", "archived"]) %>
priority: <% await tp.system.suggester(["high", "medium", "low"], ["high", "medium", "low"]) %>
source: human
---

# <% tp.file.cursor() %>

## Overview
> What is this project? One paragraph summary.

## Goals
- [ ] 

## Tech Stack
| Component | Technology |
|-----------|------------|
| Language | |
| Framework | |
| AI/ML | |
| Infrastructure | |

## Key Decisions
> Important architectural or design decisions made

### Decision 1: 
**Context:**
**Options considered:**
**Decision:**
**Rationale:**

## Progress Log

### <% tp.date.now("YYYY-MM-DD") %>
- 

## Tasks
### To Do
- [ ] 

### In Progress
- [ ] 

### Done
- [x] 

## Resources
- Documentation: 
- Repo: 
- Related projects: 

## Claude Code Integration
> Notes for when Claude Code works on this project

**Vault path:** `~/path/to/vault/03-Projects/AI-Projects/`
**Conventions:**
- Update this file's `updated` field when making changes
- Add progress entries under Progress Log
- Keep source field as `human` for manual edits, `claude-code` for automated

---

## Retrospective
> Fill out when project completes

**What went well:**
**What could improve:**
**Key learnings:**
**Brag-worthy outcomes:** [[02-Career/Brag-Doc/]]
