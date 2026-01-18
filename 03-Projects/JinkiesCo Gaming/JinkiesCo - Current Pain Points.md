---
tags:
  - jinkiesco
  - context
  - problems
created: 2026-01-17
---

# JinkiesCo - Current Pain Points

## Identified Pain Points

### 1. TCGplayer Management
- Uploading and sorting cards is time-consuming
- **Mitigation:** Roca machine helps, but only outputs to TCGplayer (not Shopify)

### 2. Price Lookups
- No stickers on items (intentional for market flexibility)
- Staff must walk to main computer for every price check
- Slows down customer interactions
- Need separate lookups for raw (TCGplayer) vs graded (eBay)

### 3. Business Taxes
- Tax season complexity with retail + online sales
- Multiple revenue streams (TCGplayer, Shopify, in-store)
- Potentially multi-state nexus from online sales

### 4. Physical Inventory Organization
- Cabinets 2 & 3 lack organization system
- Hard to find items quickly
- No tracking of what's in stock vs what sold
- No connection between physical location and digital inventory

### 5. Card Show Prep
- Pricing items night before is arduous
- Happens before every show (recurring pain)
- Need to look up each item individually

### 6. Buy/Trade Decisions
- 70% cash / 80% trade is a heuristic
- Some items are traps (illiquid, declining, oversupplied)
- No systematic way to assess liquidity
- Risk of tying up cash in items that don't move

### 7. Rip and Ship Operations
- Manual order tracking during streams
- Post-stream shipping is one label at a time
- No unified dashboard for stream operations

### 8. Security (Upcoming)
- New exterior-facing storefront
- More vulnerable to break-ins
- Need physical security investment

---

## Root Cause Analysis

```
┌─────────────────────────────────────────────────────────────────┐
│                    CORE BOTTLENECK                              │
│                                                                 │
│   Manual processes compete with content creation time           │
│   Every hour on operations = hour NOT on content                │
│   Content drives growth (166k subs in ~1 year)                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
   │ No unified  │    │ Information │    │ Manual      │
   │ inventory   │    │ scattered   │    │ repetitive  │
   │ system      │    │ across apps │    │ tasks       │
   └─────────────┘    └─────────────┘    └─────────────┘
          │                   │                   │
          ▼                   ▼                   ▼
   • Can't find items   • Price lookups   • Shipping labels
   • Don't know stock     take too long   • Card show prep
   • No cost tracking   • Multiple apps   • Buy/trade calcs
                          for raw/graded
```

---

## Pain Points by Frequency

| Pain Point | Frequency | Impact per Occurrence | Total Impact |
|------------|-----------|----------------------|--------------|
| Price lookups | Multiple times daily | Medium | **Very High** |
| Buy/trade decisions | Multiple times daily | High | **Very High** |
| Finding items | Multiple times daily | Medium | **High** |
| Card show prep | Before each show | High | **High** |
| Post-stream shipping | After each stream | Medium | **High** |
| Tax tracking | Ongoing / seasonal | High | **Medium** |
| Roca → multi-platform | After processing | Medium | **Medium** |

---

## Force Multiplier Opportunities

The family team is the constraint. Tools should:

| Goal | How |
|------|-----|
| **Distribute expertise** | Anyone can make good buy decisions with liquidity tool |
| **Reduce cognitive load** | Dashboard shows what to do, not hunting for info |
| **Eliminate walking** | Price lookups on any device at any location |
| **Batch operations** | Shipping labels all at once, not one by one |
| **Front-load work** | Card show prep automated, not night-before scramble |

---

## What's NOT a Pain Point

- ✅ Roca → TCGplayer flow works well
- ✅ Shopify handles online orders fine
- ✅ Content creation process (that's Sabrina's strength)
- ✅ Community/tournament operations

---

Back to [[JinkiesCo - Project Index]]
