---
tags:
  - jinkiesco
  - project
  - spec
  - roca
created: 2026-01-17
status: reference
---

# JinkiesCo - Roca Augmentation

## Overview

JinkiesCo has a **Roca machine at home** for card processing. This document identifies gaps in the Roca workflow and potential augmentation opportunities.

---

## What Roca Does Well

| Capability | Details |
|------------|---------|
| **Sorting** | 500-725 cards/hour autonomously |
| **Custom Sorts** | 50+ unique programmed sorts and sifts |
| **Sifting** | Separate valuable cards from bulk |
| **Alphabetizing** | Full alphabetization in single sort |
| **Pack Creator** | Create custom repacks/lots for sale |
| **Sort to Ship** | Organize cards by order number from CSV |
| **Digitization** | Creates CSV for TCGplayer import |

---

## Roca Specifications

| Model | Capacity | Speed (Sift) | Speed (Sort) |
|-------|----------|--------------|--------------|
| Roca Sorter | 1,000 cards | 725/hour | 500/hour |
| Roca Sorter Max | 3,000 cards | 450/hour | 300/hour |

**Supported Games:**
- Magic: The Gathering
- Pokémon
- Yu-Gi-Oh! (separate machine due to card size)
- Disney Lorcana
- One Piece Card Game

---

## Current Workflow

```
Large lot purchase (card show, etc.)
                ↓
Cards brought home
                ↓
Processed through Roca
├── Sift: Separate valuable from bulk
├── Sort: Alphabetize/organize
└── Digitize: Create CSV
                ↓
CSV uploaded to TCGplayer
                ↓
Cards stored/shipped
```

---

## Key Roca Limitations

| Limitation | Impact | Potential Solution |
|------------|--------|-------------------|
| **No condition grading** | Uses condition from input, doesn't assess | Pre-Roca condition tool |
| **TCGplayer-centric export** | Doesn't natively export to Shopify | Roca → Shopify bridge |
| **No cost basis tracking** | Can't calculate margins | Lot cost tracker |
| **Single game at a time (Sort to Ship)** | Can't mix PKM + MTG orders | Workflow separation |
| **At home, not at store** | Transport logistics | Home → store pipeline |
| **Batch processing** | Not real-time inventory updates | Scheduled sync |

---

## Augmentation Opportunities

### 1. Pre-Roca Condition Assessment Tool

**Problem:** Roca doesn't grade condition—garbage in, garbage out.

**Solution:** Fast condition-marking interface before Roca processing.

```
┌─────────────────────────────────────────────────┐
│  CONDITION ASSESSMENT                           │
├─────────────────────────────────────────────────┤
│                                                 │
│  Card: Charizard ex - Obsidian Flames          │
│                                                 │
│  ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐      │
│  │ NM  │ │ LP  │ │ MP  │ │ HP  │ │ DMG │      │
│  │ (1) │ │ (2) │ │ (3) │ │ (4) │ │ (5) │      │
│  └─────┘ └─────┘ └─────┘ └─────┘ └─────┘      │
│                                                 │
│  Cards graded: 127 / ~500                      │
│  ▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░                   │
│                                                 │
│  [UNDO] [SKIP]                    [VOICE MODE] │
│                                                 │
└─────────────────────────────────────────────────┘
```

**Features:**
- Keyboard shortcuts (1-5) for speed
- Optional voice commands
- Generates CSV that Roca uses
- Tracks condition distribution

**Effort:** 1-2 weeks

---

### 2. Roca → Multi-Platform Bridge

**Problem:** Roca outputs to TCGplayer only. JinkiesCo also sells on Shopify.

**Current Assessment:** Per conversation, TCGplayer and Shopify serve different purposes:
- TCGplayer = MTG singles (Roca output)
- Shopify = Sealed, Rip and Ship

**Conclusion:** Bridge may not be needed if channels remain separate. Revisit if singles go to Shopify.

---

### 3. Lot Cost Basis Tracker

**Problem:** When buying a $33k lot, how do you know if you profited?

**Solution:** Track cost through the pipeline.

```
Lot Purchase
├── Date: Jan 10, 2026
├── Source: Card Party Seattle
├── Total Cost: $33,000
├── Estimated Cards: ~5,000
└── Average Cost/Card: $6.60
                ↓
Roca Processing
├── Total Cards Scanned: 4,847
├── Total Market Value: $52,340
├── Estimated Profit: $19,340 (58% margin)
└── High Value (>$50): 23 cards
                ↓
As Cards Sell
├── Sold: 1,247 cards
├── Revenue: $18,420
├── Allocated Cost: $8,231
├── Realized Profit: $10,189
└── Remaining Inventory: 3,600 cards
```

**Content Opportunity:** "Did I Actually Profit on This $33k Lot?"

**Effort:** 3-4 weeks

---

### 4. Roca Output Analytics Dashboard

**Problem:** After processing 1,000 cards, what did you actually get?

**Solution:** Visualization dashboard.

```
┌─────────────────────────────────────────────────────────────────┐
│  ROCA RUN ANALYSIS - Jan 15, 2026                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Total Cards: 1,000          Est. Value: $4,230                │
│                                                                 │
│  VALUE DISTRIBUTION                                             │
│  ─────────────────────────────────────────────────────────────  │
│  $100+     ▓▓ 2 cards ($340)                                   │
│  $50-99    ▓▓▓▓ 8 cards ($520)                                 │
│  $20-49    ▓▓▓▓▓▓▓▓ 24 cards ($780)                            │
│  $5-19     ▓▓▓▓▓▓▓▓▓▓▓▓▓▓ 87 cards ($890)                      │
│  $1-4      ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ 234 cards ($580)                 │
│  <$1       ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ 645 cards ($320)         │
│                                                                 │
│  TOP PULLS                                                      │
│  ─────────────────────────────────────────────────────────────  │
│  1. Charizard ex SAR - $142                                    │
│  2. Pikachu VMAX Alt - $89                                     │
│  3. Umbreon V Alt - $67                                        │
│                                                                 │
│  RARITY BREAKDOWN                                               │
│  ─────────────────────────────────────────────────────────────  │
│  Special Art Rare:  3    │  Ultra Rare:    12                  │
│  Illustration Rare: 18   │  Holo Rare:     45                  │
│  Rare:              89   │  Common/Uncommon: 833               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Effort:** 2-3 weeks

---

### 5. Home → Store Inventory Pipeline

**Problem:** Roca is at home, store is at North County Mall.

**Solution:** Track what's processed, what needs transport, what arrives.

```
┌─────────────────────────────────────────────────┐
│  INVENTORY PIPELINE                             │
├─────────────────────────────────────────────────┤
│                                                 │
│  AT HOME (Roca Output)                         │
│  ├── Ready for TCGplayer: 234 cards            │
│  ├── Ready for Store: 45 cards                 │
│  └── Bulk (storage): 1,200 cards               │
│                                                 │
│  IN TRANSIT                                     │
│  └── Batch #47: 38 cards (packed Jan 15)       │
│                                                 │
│  AT STORE                                       │
│  ├── Cabinet 1: 23 items                       │
│  ├── Cabinet 2: 67 items                       │
│  └── Binders: ~500 cards                       │
│                                                 │
│  [CREATE TRANSPORT BATCH]  [CONFIRM ARRIVAL]   │
│                                                 │
└─────────────────────────────────────────────────┘
```

**Effort:** 3-4 weeks

---

## Priority Assessment

| Project | Impact | Effort | Priority |
|---------|--------|--------|----------|
| Lot Cost Tracker | High | 3-4 wks | ⭐ Build |
| Roca Output Analytics | Medium | 2-3 wks | ⭐ Build |
| Pre-Roca Condition Tool | Medium | 1-2 wks | Consider |
| Home → Store Pipeline | Medium | 3-4 wks | Later |
| Roca → Shopify Bridge | Low | 2-3 wks | Not needed currently |

---

## Integration with Other Tools

The Roca augmentation tools feed into the larger system:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  ┌─────────────┐         ┌─────────────┐                       │
│  │ Lot Cost    │────────►│ Card Show   │ (track lot           │
│  │ Tracker     │         │ Prep Tool   │  profitability)      │
│  └─────────────┘         └─────────────┘                       │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────┐         ┌─────────────┐                       │
│  │ Roca Output │────────►│ Inventory   │ (items enter         │
│  │ Analytics   │         │ System      │  with cost basis)    │
│  └─────────────┘         └─────────────┘                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Questions to Clarify

- [ ] Which Roca model? (Sorter or Sorter Max)
- [ ] Are they on Premium Software Plan? (Sort to Ship access)
- [ ] Current pain points with Roca workflow?
- [ ] How is condition currently determined before Roca?
- [ ] What happens to cards after Roca processing?

---

Back to [[JinkiesCo - Project Index]]
