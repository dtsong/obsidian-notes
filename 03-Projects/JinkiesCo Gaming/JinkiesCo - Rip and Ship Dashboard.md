---
tags:
  - jinkiesco
  - project
  - spec
  - priority-3
  - content
created: 2026-01-17
status: to-spec
effort: 3-4 weeks
---

# JinkiesCo - Rip and Ship Dashboard

> **Priority 3** - Direct revenue stream, improves every stream

## Overview

Dedicated streaming application for Rip and Ship livestreams that displays the order queue, auto-advances between customers, works as OBS overlay, and logs all opened cards.

---

## Current Workflow

```
1. Viewers purchase packs on jinkiesco.com (Shopify)
                    ↓
2. Sabrina opens packs live on stream
                    ↓
3. Manually track order numbers during stream
                    ↓
4. Cards packaged and shipped to buyers
                    ↓
5. Shipping labels created one at a time
```

### Pain Points

| Pain Point | Impact |
|------------|--------|
| Manually tracking order numbers | Cognitive load during stream |
| No visual queue display | Viewers don't know when they're up |
| Matching cards to orders | Risk of errors |
| Post-stream shipping | One label at a time is slow |
| No record of what was opened | Disputes are hard to resolve |

---

## Dashboard Interface

### Main Stream View

```
┌─────────────────────────────────────────────────────────────────┐
│                    JINKIESCO RIP AND SHIP                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    NOW RIPPING                           │   │
│  │                                                          │   │
│  │   Order #1047 - @PokeFan2025                            │   │
│  │   ─────────────────────────────────────────────         │   │
│  │   3x Surging Sparks Booster                             │   │
│  │   1x Prismatic Evolutions ETB                           │   │
│  │                                                          │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌──────────────────────┐  ┌──────────────────────┐            │
│  │      UP NEXT         │  │      ON DECK         │            │
│  │                      │  │                      │            │
│  │  #1048 - @CardKing   │  │  #1049 - @Pokemon... │            │
│  │  2x Surging Sparks   │  │  5x Surging Sparks   │            │
│  └──────────────────────┘  └──────────────────────┘            │
│                                                                 │
│  Queue: 12 orders remaining                    [NEXT ORDER →]  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Staff Control Panel (Not Shown on Stream)

```
┌─────────────────────────────────────────────────────────────────┐
│  CONTROL PANEL                              [OBS] [Settings]    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Current Order: #1047 - @PokeFan2025                           │
│  ─────────────────────────────────────────────────             │
│                                                                 │
│  Items:                                                         │
│  [✓] Surging Sparks Booster #1                                 │
│  [✓] Surging Sparks Booster #2                                 │
│  [ ] Surging Sparks Booster #3                                 │
│  [ ] Prismatic Evolutions ETB                                  │
│                                                                 │
│  Cards Pulled This Order:                                      │
│  • Pikachu ex (logged 7:42 PM)                                 │
│  • Charizard illustration rare (logged 7:43 PM)                │
│                                                                 │
│  ┌──────────────────┐  ┌──────────────────┐                    │
│  │  [NEXT ORDER]    │  │  [LOG PULL]      │                    │
│  │  Spacebar        │  │  L key           │                    │
│  └──────────────────┘  └──────────────────┘                    │
│                                                                 │
│  ┌──────────────────┐  ┌──────────────────┐                    │
│  │  [SKIP ORDER]    │  │  [PAUSE QUEUE]   │                    │
│  └──────────────────┘  └──────────────────┘                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Features

### Stream Display
- Current order prominently displayed
- Customer username/name
- What they purchased
- Up next preview
- Queue count remaining
- Clean, camera-friendly design
- OBS overlay compatible (transparent background option)

### Staff Controls
- **Keyboard shortcuts** for hands-free operation
  - `Spacebar` = Next order
  - `L` = Log notable pull
  - `P` = Pause queue
  - `S` = Skip order (with reason)
- Checklist to mark items as opened
- Quick pull logging with timestamp
- Sound effect on order advance (optional)

### Order Management
- Real-time sync with Shopify orders
- Filter to "Rip and Ship" product type only
- Auto-refresh as new orders come in
- Handle order modifications/cancellations

### Post-Stream
- Summary report: what was shipped to whom
- Batch export for shipping labels (ShipStation/Pirate Ship)
- Pull log with timestamps (for disputes)
- Analytics: popular products, stream duration, etc.

---

## Technical Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     RIP AND SHIP DASHBOARD                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐         ┌─────────────────┐               │
│  │   Stream View   │         │  Control Panel  │               │
│  │   (OBS Source)  │◄───────►│  (Staff iPad)   │               │
│  └────────┬────────┘         └────────┬────────┘               │
│           │                           │                         │
│           └───────────┬───────────────┘                         │
│                       ▼                                         │
│              ┌─────────────────┐                                │
│              │   WebSocket     │                                │
│              │   Real-time     │                                │
│              └────────┬────────┘                                │
│                       │                                         │
│                       ▼                                         │
│              ┌─────────────────┐                                │
│              │  Shopify API    │                                │
│              │  (Orders)       │                                │
│              └─────────────────┘                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend | React / Next.js |
| Real-time | WebSocket or Shopify webhooks |
| Styling | Tailwind CSS |
| Hosting | Vercel |
| API | Shopify Orders API |

---

## OBS Integration

### Browser Source Setup
1. Add Browser Source in OBS
2. Point to dashboard stream view URL
3. Set dimensions (e.g., 400x300)
4. Enable transparency
5. Position on stream layout

### Overlay Modes
- **Full panel** - Shows order details + queue
- **Minimal** - Just current order name
- **Queue only** - List of upcoming orders
- **Hidden** - Control panel only (for breaks)

---

## Shipping Integration

### During Stream
- Orders marked as "ripped" when completed

### Post-Stream
- Export all completed orders to CSV
- Compatible with:
  - **ShipStation** (direct integration possible)
  - **Pirate Ship** (CSV import)
  - **Shopify Shipping** (native)

### Batch Label Generation
```
1. End stream
            ↓
2. Click "Generate Shipping Batch"
            ↓
3. All completed orders → ShipStation/Pirate Ship
            ↓
4. Print all labels at once
            ↓
5. Pack and ship
```

---

## Pull Logging (Dispute Protection)

Every notable pull logged with:
- Timestamp
- Order number
- Card name (manual entry or quick select)
- Optional: photo capture

**Use case:** Customer claims they didn't receive a card → check pull log → see it was logged at 7:43 PM under their order.

---

## Metrics & Analytics

| Metric | Value |
|--------|-------|
| Orders per stream | Count |
| Average order value | $ |
| Stream duration | Time |
| Popular products | Ranked list |
| Peak ordering time | When orders spike |
| Notable pulls | List with timestamps |

---

## Phased Build

### Phase 1: Core Dashboard (Week 1-2)
- [ ] Shopify order fetch
- [ ] Queue display (current + upcoming)
- [ ] Next order button + keyboard shortcut
- [ ] Basic stream view (OBS compatible)

### Phase 2: Controls & Logging (Week 2-3)
- [ ] Full control panel
- [ ] Pull logging with timestamps
- [ ] Sound effects
- [ ] Order skip/pause functionality

### Phase 3: Post-Stream & Polish (Week 3-4)
- [ ] Post-stream summary report
- [ ] ShipStation/Pirate Ship export
- [ ] Analytics dashboard
- [ ] Multiple overlay modes

---

## Open Questions

- [ ] What streaming software? (OBS, Streamlabs, etc.)
- [ ] Current shipping workflow details?
- [ ] How are Rip and Ship products tagged in Shopify?
- [ ] Preferred shipping service? (ShipStation, Pirate Ship, other?)
- [ ] Need customer display name or just order number?

---

## Quick Win: Batch Shipping

Even before the full dashboard, connect **ShipStation** or **Pirate Ship** to Shopify:
- Effort: 1-2 days
- Impact: 10x faster post-stream shipping
- No custom development needed

---

Back to [[JinkiesCo - Project Index]]
