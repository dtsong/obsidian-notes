---
tags:
  - jinkiesco
  - project
  - spec
  - priority-5
created: 2026-01-17
status: to-spec
effort: 3-4 weeks
---

# JinkiesCo - Card Show Prep Tool

> **Priority 5** - Recurring pain before every show, saves hours each time

## Overview

A tool to streamline the night-before card show preparation: batch price lookups, pricing rule application, label generation, and post-show reconciliation.

---

## The Problem

Before every card show:
1. Pull items to bring
2. Look up each item's current market price
3. Decide on asking price
4. Label or note prices
5. Pack for show

**This happens before EVERY show.** They do many shows: Card Party Seattle, LA Card Expo, Front Row, Collect-A-Con, local SD shows, etc.

---

## Current Workflow Pain

| Step | Time | Pain Level |
|------|------|------------|
| Select items to bring | 30-60 min | Low |
| Price lookup (each item) | 2-4 hours | **HIGH** |
| Decide asking price | Included above | Medium |
| Create price sheet/labels | 30-60 min | Medium |
| Pack items | 30-60 min | Low |

**Total: 4-6+ hours the night before a show**

---

## Tool Workflow

### Pre-Show (Night Before)

```
1. Select items to bring
   ├── Scan barcodes (if labeled)
   ├── Search and add
   └── Import from inventory system
                    ↓
2. Batch price lookup
   ├── TCGplayer market for raw
   └── eBay recent sales for graded
                    ↓
3. Apply pricing rules
   ├── "90% of market"
   ├── "Round to nearest $5"
   └── "Floor = cost basis + 20%"
                    ↓
4. Review and adjust
   └── Override individual items as needed
                    ↓
5. Generate outputs
   ├── Price sheet (printable)
   ├── Mobile reference (phone/tablet)
   └── Optional: price labels
                    ↓
6. Pack and go!
```

### At Show

```
┌─────────────────────────────────────────────────┐
│  CARD SHOW MODE                    [Scan 📷]   │
├─────────────────────────────────────────────────┤
│                                                 │
│  Quick Lookup:                                  │
│  ┌─────────────────────────────────────────┐   │
│  │ 🔍 Search item...                       │   │
│  └─────────────────────────────────────────┘   │
│                                                 │
│  ─────────────────────────────────────────     │
│  PSA 10 Charizard ex                           │
│                                                 │
│  Market:     $485                              │
│  Your Ask:   $450                              │
│  Your Floor: $400                              │
│  Cost Basis: $320                              │
│                                                 │
│  ✅ Good margin if sold at floor               │
│  ─────────────────────────────────────────     │
│                                                 │
│  [MARK SOLD $___]  [MARK TRADED]  [PASS]       │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Post-Show

```
┌─────────────────────────────────────────────────┐
│  SHOW SUMMARY: Card Party Seattle               │
├─────────────────────────────────────────────────┤
│                                                 │
│  Items Brought:        47                       │
│  Items Sold:           28                       │
│  Items Traded:         5                        │
│  Items Returned:       14                       │
│                                                 │
│  ─────────────────────────────────────────     │
│  Total Revenue:        $3,420                  │
│  Total Cost Basis:     $2,180                  │
│  Gross Profit:         $1,240                  │
│  Margin:               36%                      │
│  ─────────────────────────────────────────     │
│                                                 │
│  Trade Value Acquired: $890                    │
│                                                 │
│  [EXPORT REPORT]  [SYNC TO INVENTORY]          │
│                                                 │
└─────────────────────────────────────────────────┘
```

---

## Features

### Pre-Show Prep

| Feature | Description |
|---------|-------------|
| Batch item selection | Scan, search, or import from inventory |
| Batch price lookup | All items priced in one operation |
| Pricing rules | Configurable (% of market, rounding, floors) |
| Manual override | Adjust individual items |
| Price sheet generation | Printable PDF |
| Mobile reference | Access on phone at show |

### At Show

| Feature | Description |
|---------|-------------|
| Quick lookup | Know your floor during negotiation |
| Sale logging | Mark items as sold with price |
| Trade logging | Mark items as traded, log what received |
| Pass logging | Track what didn't sell |
| Offline capable | Works without internet |

### Post-Show

| Feature | Description |
|---------|-------------|
| Summary report | Revenue, profit, margin |
| Inventory sync | Mark sold items, add traded items |
| Cost basis tracking | Know actual profit per show |
| Historical data | Compare show performance over time |

---

## Pricing Rules Engine

### Example Rules

```yaml
default_rules:
  raw_singles:
    base: tcgplayer_market
    multiplier: 0.90  # 90% of market
    rounding: nearest_5
    floor: cost_basis * 1.20  # At least 20% margin

  graded_slabs:
    base: ebay_average_30d
    multiplier: 0.95  # 95% of recent sales
    rounding: nearest_10
    floor: cost_basis * 1.25  # At least 25% margin

  sealed_product:
    base: tcgplayer_market
    multiplier: 0.95
    rounding: nearest_1
    floor: cost_basis * 1.15
```

### Override Options

- Set specific price per item
- Set specific floor per item
- Mark as "firm" (no negotiation)
- Mark as "flexible" (willing to deal)

---

## Integration Points

### Inventory System
- Pull items from inventory
- Update inventory post-show
- Track items at shows vs in store

### Liquidity Tool
- Flag low-liquidity items
- Suggest steeper discounts for slow movers
- Prioritize what to bring based on liquidity

### Lot Cost Tracker
- Items from specific lots trackable
- Show profit per lot across shows

---

## Price Sheet Output

### Printable Version

```
┌─────────────────────────────────────────────────────────────────┐
│  JINKIESCO - Card Party Seattle - Jan 2026                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  SLABS                                                          │
│  ─────────────────────────────────────────────────────────────  │
│  PSA 10 Charizard ex - Obsidian Flames .......... $450 (F:$400)│
│  PSA 9 Pikachu VMAX - Vivid Voltage ............. $120 (F:$100)│
│  CGC 9.5 Umbreon VMAX Alt - Evolving Skies ...... $380 (F:$340)│
│                                                                 │
│  RAW SINGLES                                                    │
│  ─────────────────────────────────────────────────────────────  │
│  Charizard ex SAR - Obsidian Flames ............. $135 (F:$115)│
│  Mew ex SAR - 151 ............................... $60  (F:$50) │
│  Pikachu IR - Surging Sparks .................... $25  (F:$20) │
│                                                                 │
│  F = Floor price for negotiation                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Technical Approach

### Offline-First

Shows may have poor internet. Tool should:
- Sync before leaving
- Work fully offline
- Sync when back online

### Mobile-Friendly

- Primary use at show is on phone
- Large touch targets
- Quick actions for logging sales

---

## Build Phases

### Phase 1: Prep Tool (Week 1-2)
- [ ] Item selection (manual entry)
- [ ] Batch price lookup
- [ ] Basic pricing rules
- [ ] Price sheet PDF generation

### Phase 2: Show Mode (Week 2-3)
- [ ] Mobile interface
- [ ] Quick lookup with floor display
- [ ] Sale/trade logging
- [ ] Offline support

### Phase 3: Post-Show (Week 3-4)
- [ ] Summary report
- [ ] Profit calculation
- [ ] Inventory sync integration
- [ ] Historical tracking

---

## Content Opportunity

> "How We Prep for Card Shows" video showing the tool in action

- Behind-the-scenes content
- Demonstrates professionalism
- Could help other sellers

---

## Open Questions

- [ ] How are items currently tracked for shows?
- [ ] What format is the current price sheet (if any)?
- [ ] How is internet connectivity at typical shows?
- [ ] Do they want to track what they BUY at shows too?

---

Back to [[JinkiesCo - Project Index]]
