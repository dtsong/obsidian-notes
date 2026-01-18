---
tags:
  - jinkiesco
  - project
  - spec
  - priority-4
created: 2026-01-17
status: to-spec
effort: 4-6 weeks
---

# JinkiesCo - Inventory System

> **Priority 4** - Foundational system that connects everything

## Overview

Zone-based inventory management that tracks physical location, integrates with Shopify, and enables quick retrieval without requiring exact slot-level tracking.

---

## Problem It Solves

- No organization system in cabinets
- Can't find items when customer asks
- No connection between physical location and digital inventory
- Sales not automatically tracked
- Staff can't easily know what's in stock

---

## Design Philosophy

### Why NOT Exact Slot Tracking

If you track exact positions (Case 1, Row 2, Slot 5):
- Must update system every time you rearrange
- Re-log positions when you sell and shift items
- High maintenance = system falls out of sync = useless

### Zone + Category System

Instead of exact slots, track **zone + organization logic**:
- Item is in "Cabinet 1 (High Value)"
- Cabinet 1 is sorted by "Price Descending"
- Staff knows: high-value item = near top of Cabinet 1

---

## Zone Definitions

```
┌─────────────────────────────────────────────────────────────────┐
│                        DISPLAY CASES                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   CAB-1         │  │   CAB-2         │  │   CAB-3         │ │
│  │   $100+         │  │   Raw singles   │  │   Raw singles   │ │
│  │   Slabs         │  │   Toploaders    │  │   Toploaders    │ │
│  │                 │  │                 │  │                 │ │
│  │   Sort: Price ↓ │  │   Sort: TBD     │  │   Sort: TBD     │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │  BIND-$1    │  │ BIND-PLAY   │  │  BIND-IR    │             │
│  │  $1 cards   │  │ Playables   │  │  $5-$30     │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │   SEAL-PKM │ SEAL-MTG │ SEAL-WS │ SEAL-OTHER            │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Inventory Tracking Levels

Not everything needs the same granularity:

| Category | Track Individually? | Barcode Each? | Notes |
|----------|:------------------:|:-------------:|-------|
| Slabs $100+ | ✅ Yes | ✅ Yes | High value, track each |
| Raw singles $30+ | ✅ Yes | ✅ Yes | Worth the effort |
| Raw singles $5-30 | ✅ Yes | ✅ Yes | Illustration rares |
| Playables $2-10 | Maybe | Optional | Could batch by card name |
| $1 binder cards | ❌ No | ❌ No | Track as bulk quantity |
| Sealed (high-end) | ✅ Yes | ✅ Yes | Track each unit |
| Sealed (low-end) | ✅ Yes | Maybe | Track quantity by SKU |

---

## Workflows

### Intake (New Item Enters Store)

```
1. Item arrives (from Roca, purchase, trade-in)
                    ↓
2. Scan/search to identify in system
                    ↓
3. Assign condition (if not already)
                    ↓
4. System suggests zone based on:
   - Game (PKM/MTG/etc)
   - Type (slab/raw/sealed)
   - Value ($100+ → CAB-1, $30-100 → CAB-2)
                    ↓
5. Print barcode label (SKU, no price)
                    ↓
6. Staff places in correct zone, following sort rule
                    ↓
7. Item is now findable + sellable
```

### Sale

```
1. Customer wants item
                    ↓
2. Staff scans barcode OR searches system
                    ↓
3. System shows: 
   - Location (zone)
   - Current market price
   - Suggested sell price
                    ↓
4. Staff retrieves from zone (using sort logic)
                    ↓
5. Scan at checkout → Shopify POS
                    ↓
6. Inventory decrements, zone updated
```

### Lookup (Staff View)

```
┌─────────────────────────────────────────────────┐
│ PSA 10 Charizard ex (Obsidian Flames)          │
├─────────────────────────────────────────────────┤
│ Location:     CAB-1 (High Value $100+)         │
│ Sort Rule:    Price descending                  │
│ Market Price: $850                              │
│ Our Price:    $799                              │
│                                                 │
│ → Look near TOP of Cabinet 1 (high value)      │
└─────────────────────────────────────────────────┘
```

---

## Barcode Label System

### Label Placement

| Item Type | Label Placement |
|-----------|-----------------|
| Slabs | Small label on back or bottom edge of case |
| Toploaded singles | Label on toploader (not the card) |
| Binder singles | Label on sleeve or small tag in pocket |
| Sealed product | Label on product or shelf tag |

### Label Contents

```
┌─────────────────────────┐
│ ▮▮▮▮▮▮▮▮▮▮▮▮▮▮▮▮▮▮▮▮▮ │
│ JKC-PKM-00001          │
│ (NO PRICE ON LABEL)    │
└─────────────────────────┘
```

- SKU/barcode only
- No price (intentional)
- Scannable from any device

---

## System Features

| Feature | Purpose |
|---------|---------|
| Item database | All inventory with SKU, condition, cost basis |
| Zone assignment | Each item tagged to a zone |
| Zone rules | Each zone has sort logic |
| Barcode scanning | Scan item → see details, initiate sale |
| Shopify sync | Sale in store → decrements Shopify inventory |
| Intake workflow | Add new items quickly, print label, assign zone |
| Price lookup | Scan → see market price, suggested sell price |
| Low stock alerts | Notify when zone gets empty |

---

## Integration Points

```
┌─────────────────────────────────────────────────────────────────┐
│                     INVENTORY SYSTEM                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐         │
│  │ Price       │◄──►│ Inventory   │◄──►│ Buy/Trade   │         │
│  │ Lookup App  │    │ Database    │    │ Tool        │         │
│  └─────────────┘    └──────┬──────┘    └─────────────┘         │
│                            │                                    │
│                            ▼                                    │
│                    ┌─────────────┐                              │
│                    │  Shopify    │                              │
│                    │  POS + API  │                              │
│                    └─────────────┘                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Build vs Buy

| Option | Pros | Cons |
|--------|------|------|
| **Shopify POS + barcodes** | Native, simple | No TCG features, no location tracking |
| **BinderPOS** | Built for TCG, Shopify sync | $100-300/mo, learning curve |
| **SortSwift** | Modern, flat fee | Newer platform |
| **Custom build** | Exactly what they need | Dev time, maintenance |

### Recommendation

1. Start with **Shopify POS** + barcode labels for basics
2. **Custom layer** adds:
   - Zone/location tracking
   - Market price lookup at scan
   - Liquidity assessment at intake
   - Integration with buy/trade tool

---

## Phased Build

### Phase 1: Foundation
- [ ] Barcode labels + scanner hardware
- [ ] Zone definitions in system
- [ ] Basic item database with zone tracking
- [ ] Shopify POS integration for sales
- [ ] Simple intake workflow

### Phase 2: Intelligence
- [ ] Integrate with [[JinkiesCo - Price Lookup App]]
- [ ] Integrate with [[JinkiesCo - Liquidity Assessment Tool]]
- [ ] Market price lookup at scan
- [ ] Buy/trade workflow

### Phase 3: Polish
- [ ] Customer-facing kiosk (browse digitally)
- [ ] Analytics (what's selling, what's sitting)
- [ ] Low stock alerts by zone

---

## Hardware Needed

| Item | Purpose | Est. Cost |
|------|---------|-----------|
| Barcode scanner (USB) | Checkout scanning | $30-50 |
| Label printer | Print SKU labels | $100-200 |
| Labels/supplies | Ongoing | $20/mo |
| iPad(s) | Mobile lookup/intake | $300-500 each |

---

## Open Questions

- [ ] Current POS system? (Square, Shopify POS, other?)
- [ ] How many items to initially catalog?
- [ ] Backlog approach: label everything first, or as-you-go?
- [ ] Budget for hardware?

---

Back to [[JinkiesCo - Project Index]]
