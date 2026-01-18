---
tags:
  - jinkiesco
  - context
  - inventory
  - layout
created: 2026-01-17
---

# JinkiesCo - Store Physical Layout

## Store Layout Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           JINKIESCO STORE LAYOUT                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐             │
│  │   CABINET 1     │  │   CABINET 2     │  │   CABINET 3     │             │
│  │   High Value    │  │   Mixed         │  │   Mixed         │             │
│  │   $100+         │  │   Raw singles   │  │   Raw singles   │             │
│  │                 │  │   in toploaders │  │   in toploaders │             │
│  │   • Slabs       │  │   Some older    │  │   Some older    │             │
│  │   • High-end    │  │   sealed        │  │   sealed        │             │
│  │     singles     │  │                 │  │                 │             │
│  │                 │  │   NO PRICE      │  │   NO PRICE      │             │
│  │   NO PRICE      │  │   LABELS        │  │   LABELS        │             │
│  │   LABELS        │  │   (intentional) │  │   (intentional) │             │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘             │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                          BINDERS                                     │   │
│  │  ┌───────────┐  ┌───────────────┐  ┌─────────────────────┐          │   │
│  │  │ $1 Cards  │  │ Playables     │  │ Illustration Rares  │          │   │
│  │  │           │  │ (for players) │  │ $5-$30              │          │   │
│  │  │           │  │               │  │ (nice binder cards) │          │   │
│  │  └───────────┘  └───────────────┘  └─────────────────────┘          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                     SEALED PRODUCT SHELVES                           │   │
│  │                                                                      │   │
│  │   Pokemon │ Magic │ Weiss │ Riftbound │ Union Arena │ Other          │   │
│  │                                                                      │   │
│  │   Lower-end: PRICE LABELS                                           │   │
│  │   Higher-end: NO LABELS (lookup at inquiry)                         │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Zone Definitions

| Zone ID | Location | Contents | Price Labels? | Organization Logic |
|---------|----------|----------|---------------|-------------------|
| **CAB-1** | Cabinet 1 | High value $100+ (slabs, chase cards) | No | By value descending |
| **CAB-2** | Cabinet 2 | Raw singles in toploaders, some older sealed | No | TBD |
| **CAB-3** | Cabinet 3 | Raw singles in toploaders, some older sealed | No | TBD |
| **BIND-$1** | Binder(s) | $1 cards | No (implied $1) | TBD |
| **BIND-PLAY** | Binder(s) | Playable cards for players | No | TBD |
| **BIND-IR** | Binder(s) | Illustration rares $5-$30 | No | TBD |
| **SEAL-PKM** | Shelf | Pokemon sealed | Lower-end yes, high-end no | By product type |
| **SEAL-MTG** | Shelf | Magic sealed | Lower-end yes, high-end no | By product type |
| **SEAL-WS** | Shelf | Weiss Schwarz sealed | Lower-end yes, high-end no | By product type |
| **SEAL-OTHER** | Shelf | Riftbound, Union Arena, etc. | Lower-end yes, high-end no | By game → product |

---

## No-Price-Label Strategy

This is **intentional** for high-value items:

| Benefit | Explanation |
|---------|-------------|
| Prevents lowballing | Customer can't pre-research exact price |
| Forces conversation | Staff controls the narrative |
| Prices flex with market | No outdated stickers |
| Negotiation leverage | Staff knows floor, customer doesn't |

**System requirement:** Staff needs fast price lookup without showing customer the screen.

---

## Display Model

**Jewelry store approach:**
- Items displayed in cases for customers to physically see
- Customer points at item → staff retrieves and quotes price
- Actual card is the display (not a placeholder)

**Exception for very high value ($500+):**
- Consider photo placeholder in case
- Actual card in safe
- Retrieve for serious buyers only

---

## Current Pain Points

1. **No organization system** in Cabinets 2 & 3
2. **Can't quickly find items** when customer asks
3. **No inventory tracking** tied to physical location
4. **Price lookups require going to main computer**
5. **No barcode/SKU system** for quick identification

---

## Questions to Resolve

- [ ] What organization logic should Cabinets 2 & 3 have?
- [ ] How are binders currently sorted?
- [ ] Roughly how many items in each zone?
- [ ] How often do new items get added?

---

Back to [[JinkiesCo - Project Index]]
