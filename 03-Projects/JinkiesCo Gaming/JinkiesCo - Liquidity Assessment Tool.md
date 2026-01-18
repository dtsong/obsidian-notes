---
tags:
  - jinkiesco
  - project
  - spec
  - priority-2
created: 2026-01-17
status: to-spec
effort: 2-3 weeks
---

# JinkiesCo - Liquidity Assessment Tool

> **Priority 2** - Protects cash flow, enables anyone to make good buy decisions

## Overview

A decision support tool that helps staff determine if an item is safe to buy/trade at standard rates (70% cash / 80% trade) or if they should adjust the offer based on liquidity risk.

---

## Problem It Solves

The 70/80% rule works most of the time, but some items are traps:
- Cards that spiked recently but are crashing
- Niche items with low demand (sits in inventory for months)
- Items they already have too many of
- High-value items with thin markets (hard to move quickly)

**Result:** Cash tied up in illiquid inventory, drag on the business.

---

## User Interface

### Good Buy Example

```
┌─────────────────────────────────────────────────┐
│ Charizard ex (Obsidian Flames) - NM             │
├─────────────────────────────────────────────────┤
│ Market Price:        $48.00                     │
│ 70% Cash Offer:      $33.60                     │
│ 80% Trade Offer:     $38.40                     │
├─────────────────────────────────────────────────┤
│ Liquidity:           ●●●●○ High                 │
│ Trend:               → Stable                   │
│ Volatility:          Low                        │
│ Current Stock:       2 in inventory             │
├─────────────────────────────────────────────────┤
│ Recommendation:      ✅ BUY at 70/80%           │
└─────────────────────────────────────────────────┘
```

### Risky Buy Example

```
┌─────────────────────────────────────────────────┐
│ [Obscure Alt Art] - NM                          │
├─────────────────────────────────────────────────┤
│ Market Price:        $85.00                     │
│ 70% Cash Offer:      $59.50                     │
│ 80% Trade Offer:     $68.00                     │
├─────────────────────────────────────────────────┤
│ Liquidity:           ●○○○○ Very Low             │
│ Trend:               ↓ Declining (-15% 30d)     │
│ Volatility:          High                       │
│ Current Stock:       1 in inventory             │
├─────────────────────────────────────────────────┤
│ ⚠️  LOW LIQUIDITY + DECLINING                  │
│ Suggested:           50-60% or PASS             │
└─────────────────────────────────────────────────┘
```

---

## Liquidity Scoring

### Score Levels

| Score | Indicator | Meaning | Recommendation |
|-------|-----------|---------|----------------|
| 5 | ●●●●● | Very High | ✅ Buy at 70/80% confidently |
| 4 | ●●●●○ | High | ✅ Buy at 70/80% |
| 3 | ●●●○○ | Medium | ⚠️ Consider 60/70% |
| 2 | ●●○○○ | Low | ⚠️ Consider 50/60% |
| 1 | ●○○○○ | Very Low | ❌ Pass or 40/50% max |

### Factors That Determine Liquidity

| Factor | Weight | Data Source | Logic |
|--------|--------|-------------|-------|
| **Sales Velocity** | High | TCGplayer recent sales | More sales = more liquid |
| **Market Depth** | High | Listings vs sales ratio | Many listings + few sales = oversupplied |
| **Price Trend** | Medium | 30/90 day price history | Declining = risky |
| **Volatility** | Medium | Price swing amplitude | Wild swings = unpredictable |
| **Current Stock** | Low | Internal inventory | Already have 5? Don't need more |
| **Days to Sell** | Medium | Historical or benchmark | How long until this moves? |

### Calculation Example

```
Charizard ex (Obsidian Flames):
├── Sales Velocity:  85 sales/week on TCGplayer    → +2.0
├── Market Depth:    120 listings, 85 sales        → +1.5
├── Price Trend:     +2% over 30 days (stable)     → +0.5
├── Volatility:      5% price range (low)          → +0.5
├── Current Stock:   2 in inventory                → +0.0
└── Total Score:     4.5 → ●●●●○ High Liquidity
```

---

## Price Trend Indicators

| Indicator | Meaning |
|-----------|---------|
| ↑ Rising (+10%+ 30d) | Price going up |
| ↗ Slightly Up (+2-10%) | Modest gains |
| → Stable (-2% to +2%) | Holding steady |
| ↘ Slightly Down (-2-10%) | Modest decline |
| ↓ Declining (-10%+ 30d) | Price dropping |
| ⚠️ Volatile | Big swings both ways |

---

## Workflow Integration

### At Counter (Buy/Trade)

```
Customer: "I want to sell this Charizard"
                    ↓
Staff: Scans card or searches
                    ↓
System shows:
├── Market price
├── 70/80% offers
├── Liquidity score
└── Recommendation
                    ↓
Staff: Makes informed offer
```

### At Intake (After Trade/Purchase)

```
Item acquired
        ↓
System logs:
├── Cost basis (what we paid)
├── Liquidity at time of purchase
└── Expected margin
```

### At Card Shows

Same tool on mobile:
- Quick liquidity check before buying
- Know your floor when negotiating
- Avoid overpaying for illiquid items

---

## Suggested Offer Adjustments

| Liquidity | Cash Offer | Trade Offer |
|-----------|------------|-------------|
| Very High | 70% | 80% |
| High | 70% | 80% |
| Medium | 60% | 70% |
| Low | 50% | 60% |
| Very Low | 40% or PASS | 50% or PASS |

**Override option:** Staff can always adjust based on:
- Customer relationship
- Negotiation
- Strategic need (completing a set, etc.)

---

## Integration with Other Tools

```
┌─────────────────────────────────────────────────────────────────┐
│                   LIQUIDITY TOOL INTEGRATIONS                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐                                           │
│  │  Price Lookup   │──► Liquidity score shown with every       │
│  │  App            │    price lookup                           │
│  └─────────────────┘                                           │
│                                                                 │
│  ┌─────────────────┐                                           │
│  │  Inventory      │──► Liquidity assessed at intake           │
│  │  System         │    Flags slow-moving inventory            │
│  └─────────────────┘                                           │
│                                                                 │
│  ┌─────────────────┐                                           │
│  │  Card Show      │──► Pre-show: flag low-liquidity items     │
│  │  Prep Tool      │    At show: quick liquidity checks        │
│  └─────────────────┘                                           │
│                                                                 │
│  ┌─────────────────┐                                           │
│  │  Lot Cost       │──► Post-lot analysis: what % was          │
│  │  Tracker        │    high vs low liquidity?                 │
│  └─────────────────┘                                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Build Approach

### Phase 1: Core Scoring (Week 1-2)
- [ ] TCGplayer API integration
- [ ] Sales velocity calculation
- [ ] Market depth calculation
- [ ] Basic liquidity score output

### Phase 2: Trend & Volatility (Week 2-3)
- [ ] Price history tracking
- [ ] Trend calculation (30/90 day)
- [ ] Volatility calculation
- [ ] Full liquidity score with all factors

### Phase 3: Recommendations (Week 3)
- [ ] Offer adjustment logic
- [ ] Recommendation display
- [ ] Integration with Price Lookup App

---

## Data Sources

| Data | Source | API |
|------|--------|-----|
| Market price | TCGplayer | TCGplayer API |
| Recent sales | TCGplayer | TCGplayer API |
| Active listings | TCGplayer | TCGplayer API |
| Price history | TCGplayer or cache | TCGplayer API |
| Current stock | Internal | Inventory System |

---

## Force Multiplier Effect

> **Anyone can make good buy decisions** without needing Sabrina's expertise on every card.

| Without Tool | With Tool |
|--------------|-----------|
| "Is this card worth buying?" → Ask Sabrina | Check liquidity score |
| Mental math on 70/80% | Auto-calculated |
| Gut feeling on market trends | Data-driven trend indicator |
| Risk of bad buys | Flagged before purchase |

---

## Open Questions

- [ ] TCGplayer API access level?
- [ ] Historical price data availability?
- [ ] Internal inventory system integration timing?
- [ ] Acceptable latency for lookups?

---

Back to [[JinkiesCo - Project Index]]
