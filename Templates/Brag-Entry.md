---
date: <% tp.date.now("YYYY-MM-DD") %>
tags: brag, win
category: <% await tp.system.suggester(["technical", "leadership", "collaboration", "learning", "impact", "process-improvement"], ["technical", "leadership", "collaboration", "learning", "impact", "process-improvement"]) %>
project: 
quarter: <% tp.date.now("YYYY") %>-Q<% Math.ceil((tp.date.now("M")) / 3) %>
---

# <% tp.file.cursor() %>

## Summary
> One-liner describing the accomplishment

## Context
> What was the situation or problem?

## Action
> What did you specifically do?

## Result
> What was the outcome? Include metrics if possible.

## Skills Demonstrated
- 

## Related Links
- Daily note: [[01-Daily/<% tp.date.now("YYYY-MM-DD") %>]]
- Project: [[03-Projects/]]

---

## Performance Review Snippet
> Pre-write how you'd describe this in a review (2-3 sentences)

