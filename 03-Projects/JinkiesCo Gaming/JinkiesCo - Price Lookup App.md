---
tags:
  - jinkiesco
  - project
  - spec
  - priority-1
created: 2026-01-17
status: to-spec
effort: 1-2 weeks
---

# JinkiesCo - Price Lookup App

> **Priority 1** - Quick win with immediate daily value

## Overview

Simple web app that works on any device (iPads, phones, tablets) for instant price lookups. No app store install needed—just bookmark it.

---

## Problem It Solves

- Staff must walk to main computer for every price check
- Slows down customer interactions at cabinets
- Need separate lookups for raw (TCGplayer) vs graded (eBay)
- Buy/trade calculations done mentally or on paper
- No way to assess if an item is safe to buy at standard rates

---

## User Interface

### Search Screen

```
┌─────────────────────────────────────────────────┐
│  JinkiesCo Price Check            [Scan 📷]    │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌─────────────────────────────────────────┐   │
│  │ 🔍 Search card name, set, or scan...    │   │
│  └─────────────────────────────────────────┘   │
│                                                 │
│  [Pokemon] [Magic] [One Piece] [Other]         │
│                                                 │
│  Recent Lookups:                               │
│  • Charizard ex SAR - Obsidian Flames    $142 │
│  • Pikachu VMAX Rainbow - Vivid          $89  │
│  • Mew ex SAR - 151                      $67  │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Raw Card Result

```
┌─────────────────────────────────────────────────┐
│  ← Back                                         │
├─────────────────────────────────────────────────┤
│                                                 │
│  Charizard ex SAR                              │
│  Obsidian Flames #223                          │
│  ─────────────────────────────────────────     │
│                                                 │
│  TCGplayer Market:        $142.00              │
│  TCGplayer Low:           $134.99              │
│  Last Sold:               $138.50              │
│                                                 │
│  ─────────────────────────────────────────     │
│  SUGGESTED SELL:          $135.00              │
│  FLOOR (negotiation):     $115.00              │
│  ─────────────────────────────────────────     │
│                                                 │
│  70% Cash Offer:          $99.40               │
│  80% Trade Offer:         $113.60              │
│                                                 │
│  Liquidity: ●●●●○ High                        │
│  Trend: → Stable                               │
│                                                 │
│  [Copy Price] [Add to Cart] [Buy/Trade Calc]   │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Graded Card Result

```
┌─────────────────────────────────────────────────┐
│  ← Back                         [Raw] [Graded] │
├─────────────────────────────────────────────────┤
│                                                 │
│  Charizard ex SAR - PSA 10                     │
│  Obsidian Flames #223                          │
│  ─────────────────────────────────────────     │
│                                                 │
│  eBay Recent Sales (PSA 10):                   │
│  • $485 - Jan 14 (Best Offer Accepted)         │
│  • $510 - Jan 12                               │
│  • $475 - Jan 10 (Best Offer Accepted)         │
│  • $525 - Jan 8                                │
│                                                 │
│  Average (30 day):        $498.00              │
│  ─────────────────────────────────────────     │
│  SUGGESTED SELL:          $485.00              │
│  FLOOR:                   $425.00              │
│  ─────────────────────────────────────────     │
│                                                 │
│  70% Cash Offer:          $348.60              │
│  80% Trade Offer:         $398.40              │
│                                                 │
│  Liquidity: ●●●○○ Medium                      │
│  (12 sales in 30 days)                         │
│                                                 │
└─────────────────────────────────────────────────┘
```

---

## Features

### Phase 1: MVP (Week 1-2)

| Feature | Description |
|---------|-------------|
| **Card search** | Type-ahead suggestions as you type |
| **Game filter** | Pokemon, Magic, One Piece, etc. |
| **TCGplayer price** | Market price, low, recent sales |
| **70/80% calculator** | Instant buy/trade offer calculation |
| **Recent lookups** | Quick access to cards you just checked |
| **Mobile responsive** | Works on any screen size |

### Phase 2: Intelligence (Week 3)

| Feature | Description |
|---------|-------------|
| **eBay integration** | Recent sold prices for graded cards |
| **Raw/Graded toggle** | Switch between pricing sources |
| **Liquidity score** | Based on sales velocity, market depth |
| **Price trend** | Stable / Rising / Falling indicator |
| **Suggested sell/floor** | Calculated pricing guidance |

### Phase 3: Integration (When Ready)

| Feature | Description |
|---------|-------------|
| **Barcode scanning** | Scan labeled items for instant lookup |
| **In-stock indicator** | Check if item is in inventory |
| **Direct checkout** | Add to Shopify cart from lookup |
| **Cost basis lookup** | For items in system, show margin |

---

## Key Features Detail

### Liquidity Score

| Score | Meaning | Buy Recommendation |
|-------|---------|-------------------|
| ●●●●● Very High | Sells within days | ✅ Buy at 70/80% |
| ●●●●○ High | Sells within 1-2 weeks | ✅ Buy at 70/80% |
| ●●●○○ Medium | Sells within a month | ⚠️ Consider 60/70% |
| ●●○○○ Low | May sit for months | ⚠️ Consider 50/60% or pass |
| ●○○○○ Very Low | Hard to move | ❌ Pass or lowball (40/50%) |

### Liquidity Factors

| Factor | Data Source |
|--------|-------------|
| Sales velocity | TCGplayer recent sales count |
| Market depth | Listings vs sales ratio |
| Price trend | 30/90 day price movement |
| Volatility | Price swing amplitude |

---

## Technical Architecture

```
┌─────────────────────────────────────────┐
│         Price Lookup Web App            │
│         (React / Next.js)               │
│         Mobile-responsive PWA           │
└──────────────────┬──────────────────────┘
                   │
       ┌───────────┴───────────┐
       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐
│  TCGplayer API  │    │   eBay API      │
│  (raw cards)    │    │   (graded/slabs)│
└─────────────────┘    └─────────────────┘
```

### Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend | React / Next.js |
| Styling | Tailwind CSS |
| Hosting | Vercel (free tier) |
| APIs | TCGplayer, eBay Browse API |
| Camera | Browser native camera API |

### API Considerations

| API | Notes |
|-----|-------|
| **TCGplayer** | Requires seller account API access, rate limits apply |
| **eBay Browse** | Free tier available, need to parse sold listings |
| **Scryfall** | Free, good for MTG card data (supplement) |
| **Pokemon TCG API** | Free, good for Pokemon card data (supplement) |

---

## Deployment

1. **No app store needed** - Progressive Web App (PWA)
2. **Install to home screen** - Acts like native app
3. **Works offline** - Cache recent lookups
4. **Auto-updates** - No manual update process

---

## Who Uses It

| Who | Use Case |
|-----|----------|
| Staff at cabinets | Quick price quotes without walking |
| Staff at register | Verify prices during checkout |
| At card shows | Mobile price checks while buying/selling |
| During buy/trade | Instant 70/80% calculations with liquidity |
| Training new help | Less expertise needed to quote prices |

---

## Success Metrics

- [ ] Price lookup takes < 5 seconds
- [ ] Works on all staff devices
- [ ] No walking to main computer needed
- [ ] Buy/trade decisions include liquidity check
- [ ] Usable at card shows on mobile

---

## Open Questions

- [ ] TCGplayer API access - do they have developer credentials?
- [ ] eBay API access - need to set up developer account?
- [ ] What devices will staff use? (iPads, iPhones, Android?)
- [ ] Need offline support or always-connected OK?

---

## Next Steps

1. [ ] Confirm API access (TCGplayer, eBay)
2. [ ] Set up development environment
3. [ ] Build search + TCGplayer lookup (Phase 1)
4. [ ] Test on staff devices
5. [ ] Add eBay integration (Phase 2)
6. [ ] Add liquidity scoring (Phase 2)

---

Back to [[JinkiesCo - Project Index]]
