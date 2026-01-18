---

tags:

- jinkiesco
- project
- spec
- priority-1
- mtg
- distribution created: 2026-01-17 status: in-progress effort: 2-3 weeks

---

# JinkiesCo - MTG Commander ROI Tool

> **Priority 1** - Enables confident distributor purchasing, protects allocation

## Overview

A web-based tool that analyzes MTG Commander preconstructed decks by comparing current card market values against MSRP, helping JinkiesCo make data-driven purchasing decisions from distributors.

---

## The Strategic Problem

### Distributor Allocation Game

```
┌─────────────────────────────────────────────────────────────────┐
│                    DISTRIBUTOR RELATIONSHIP                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Account Spend History ──► Allocation on New Releases           │
│                                                                  │
│   Low spend = Limited allocation on hot products                 │
│   High spend = Priority account, full allocation                 │
│                                                                  │
│   Distributors: PeachState, GTS, etc.                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### The MTG Challenge

- Pokemon moves fast → easy to spend confidently
- MTG moves slower → risk of cash tied up in dead inventory
- BUT: Need MTG spend to maintain overall account standing
- **Solution:** Identify MTG products that WILL move and have positive ROI

---

## What It Does

|Function|Description|
|---|---|
|**ROI Calculation**|Compare current market value vs MSRP|
|**Card Price Lookup**|Real-time prices from Scryfall API|
|**Precon Database**|2022-2026 Commander precons pre-loaded|
|**Top Value Cards**|Highlight the money cards in each deck|
|**Deck Comparison**|Side-by-side evaluation of multiple decks|
|**Custom Decks**|Add new releases as they come out|

---

## User Interface

### Current State (In Development)

```
┌─────────────────────────────────────────────────────────────────┐
│  MTG Commander ROI                    [Compare] [+ Add Deck]    │
├──────────────────┬──────────────────────────────────────────────┤
│  Select Deck     │  ROI Summary                                 │
│                  │                                              │
│  Filter by Year  │  ┌─────────────────────────────────────┐    │
│  [2024 ▼]        │  │  DECK NAME                          │    │
│                  │  │  Set | MSRP: $49.99                 │    │
│  Filter by Set   │  │                                     │    │
│  [All Sets ▼]    │  │  Current Value: $67.42              │    │
│                  │  │  ROI: +34.9%  ▲                     │    │
│  • Deck 1        │  │                                     │    │
│  • Deck 2        │  └─────────────────────────────────────┘    │
│  • Deck 3        │                                              │
│                  │  Search Cards | Bulk Import                  │
│                  │                                              │
│                  │  All Cards (sorted by value)                 │
│                  │                                              │
└──────────────────┴──────────────────────────────────────────────┘
```

---

## Technical Stack

|Component|Technology|
|---|---|
|Frontend|React 18|
|Styling|Tailwind CSS|
|Icons|Lucide React|
|API|Scryfall (free, rate-limited)|
|Hosting|Local dev → Vercel (planned)|

---

## Data Sources

### Scryfall API

- **Base URL:** `https://api.scryfall.com`
- **Rate Limit:** 100ms between requests
- **Price Fields:** `prices.usd`, `prices.usd_foil`
- **Set Codes:** Commander deck sets (dsc, blc, m3c, otc, pip, mkc, etc.)

### Precon Database (Built-in)

|Year|Sets|Decks|
|---|---|---|
|2026|Lorwyn Eclipsed|2|
|2025|TBD|TBD|
|2024|Duskmourn, Bloomburrow, MH3, Thunder Junction, Fallout, Karlov Manor|24|
|2023|Lost Caverns, Doctor Who, Eldraine, Commander Masters, LOTR|18|
|2022|Warhammer 40k|4|

---

## Key Metrics

### ROI Calculation

```javascript
ROI = ((Current Value - MSRP) / MSRP) * 100

// Example:
// MSRP: $49.99
// Current Value: $67.42
// ROI: +34.9%
```

### Planned: Velocity Score

```
Velocity = How fast does this product sell?

Fast (●●●●●)   = Sells in < 7 days
Good (●●●●○)   = Sells in 7-14 days  
Medium (●●●○○) = Sells in 14-30 days
Slow (●●○○○)   = Sells in 30-60 days
Dead (●○○○○)   = Sits for 60+ days
```

### Planned: Distributor ROI

```
Distro ROI = ((Current Value - Distro Cost) / Distro Cost) * 100

// Example:
// Distro Cost: $28.00 (typical ~45% off MSRP)
// Current Value: $67.42
// Distro ROI: +140.8%  ← Real margin for store
```

---

## Use Cases

### 1. Distributor Order Planning

> "Distro order is due Friday. Which Commander precons should we add?"

Tool shows: Top ROI decks that move fast → confident ordering

### 2. Card Show Buying

> "Vendor has Fallout precons for $45. Good deal?"

Quick lookup: Check ROI at that price point

### 3. Content Creation

> "Which precons are actually worth it in 2024?"

Data-driven content → views + informed audience + sales

### 4. Inventory Decisions

> "We have 6 of this precon. Should we crack them or sell sealed?"

Compare: Sealed value vs singles value

---

## Integration Points

```
┌─────────────────────────────────────────────────────────────────┐
│                    TOOL ECOSYSTEM                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────┐         ┌─────────────────┐                │
│  │ MTG Commander   │         │ Liquidity       │                │
│  │ ROI Tool        │◄───────►│ Assessment      │                │
│  │ (sealed buying) │         │ (singles buying)│                │
│  └────────┬────────┘         └─────────────────┘                │
│           │                                                      │
│           ▼                                                      │
│  ┌─────────────────┐         ┌─────────────────┐                │
│  │ Price Lookup    │         │ Inventory       │                │
│  │ App             │◄───────►│ System          │                │
│  └─────────────────┘         └─────────────────┘                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Build Status

### Completed ✅

- [x] React app scaffolded
- [x] Precon database (2022-2026)
- [x] Year/Set filtering
- [x] Deck list display with color identity
- [x] Search Cards panel
- [x] Bulk Import panel
- [x] Add Custom Deck modal
- [x] Compare Decks button
- [x] Scryfall attribution

### In Progress 🔄

- [ ] Deck selection → card loading
- [ ] ROI calculation display
- [ ] Top value cards highlight

### Planned 📋

- [ ] Velocity/Days-to-Sell metric
- [ ] Distributor cost field
- [ ] Price caching (24hr)
- [ ] Comparison view
- [ ] Mobile responsiveness
- [ ] Deploy to Vercel

---

## Future Expansion

### Phase 2: Other MTG Sealed

- Set Booster Boxes
- Collector Booster Boxes
- Bundles

### Phase 3: Other TCGs

- Pokemon ETBs, Booster Boxes
- One Piece sealed
- Lorcana sealed
- Weiss Schwarz

---

## Files

|File|Location|Purpose|
|---|---|---|
|SPEC.md|Project root|Technical specification|
|FEEDBACK.md|Project root|Enhancement requests for Claude Code|
|App.jsx|src/|Main React component|
|precons.js|src/data/|Precon database|

---

## Open Questions

- [x] What distributor(s) does JinkiesCo use? → PeachState, GTS
- [ ] What's the typical distro discount? (40-45% assumed)
- [ ] Target spend levels to maintain allocation?
- [ ] Priority for expanding to other TCGs?

---

Back to [[JinkiesCo - Project Index]]