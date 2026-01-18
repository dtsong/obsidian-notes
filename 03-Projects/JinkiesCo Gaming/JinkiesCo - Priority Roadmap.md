---
tags:
  - jinkiesco
  - roadmap
  - planning
created: 2026-01-17
---

# JinkiesCo - Priority Roadmap

## Priority Summary

| Rank | Project | Effort | Impact | Status |
|------|---------|--------|--------|--------|
| 1 | [[JinkiesCo - Price Lookup App\|Price Lookup App]] | 1-2 weeks | Very High | 📋 To Build |
| 2 | Batch Shipping Integration | 1-2 days | High | 📋 To Do |
| 3 | [[JinkiesCo - Rip and Ship Dashboard\|Rip and Ship Dashboard]] | 3-4 weeks | Very High | 📋 To Spec |
| 4 | [[JinkiesCo - Inventory System\|Inventory System]] | 4-6 weeks | High | 📋 To Spec |
| 5 | Tax Automation | 1 week | High | 📋 To Do |
| 6 | [[JinkiesCo - Liquidity Assessment Tool\|Liquidity Tool]] | 2-3 weeks | High | 📋 To Spec |
| 7 | [[JinkiesCo - Card Show Prep Tool\|Card Show Prep Tool]] | 3-4 weeks | Medium-High | 📋 To Spec |
| 8 | [[JinkiesCo - Roca Augmentation\|Lot Cost Tracker]] | 3-4 weeks | Medium | 📋 To Spec |

---

## Detailed Roadmap

### Phase 1: Quick Wins (Week 1-2)

**Goal:** Immediate value with minimal effort

| Task | Owner | Time | Dependencies |
|------|-------|------|--------------|
| Build Price Lookup App MVP | Dev | 1-2 weeks | TCGplayer API access |
| Connect ShipStation/Pirate Ship to Shopify | Anyone | 1-2 days | Shopify admin access |
| Set up price lookup tablet at counter | Anyone | 1 day | Tablet + app |
| Document current workflows | Team | 2-3 hours | None |

**Deliverables:**
- [ ] Price Lookup App live and usable
- [ ] Batch shipping working for Rip and Ship
- [ ] Staff can look up prices from anywhere in store

---

### Phase 2: Foundation (Week 3-6)

**Goal:** Build core infrastructure

| Task | Owner | Time | Dependencies |
|------|-------|------|--------------|
| Add eBay integration to Price Lookup | Dev | 1 week | eBay API access |
| Add Liquidity scoring | Dev | 1-2 weeks | Price Lookup MVP |
| Connect TaxJar/TaxCloud to Shopify | Anyone | 1 week | Shopify access |
| Define zone organization for store | Team | 2-3 hours | None |
| Procure barcode hardware | Anyone | 1 week | Budget approval |

**Deliverables:**
- [ ] Price Lookup with graded card support + liquidity
- [ ] Sales tax automated
- [ ] Hardware ready for inventory system
- [ ] Store zones defined

---

### Phase 3: Rip and Ship (Week 7-10)

**Goal:** Streamline streaming operations

| Task | Owner | Time | Dependencies |
|------|-------|------|--------------|
| Build Rip and Ship Dashboard MVP | Dev | 2 weeks | Shopify API |
| Add OBS integration | Dev | 1 week | Dashboard MVP |
| Add pull logging | Dev | 3-4 days | Dashboard MVP |
| Test during actual stream | Team | 1 stream | Dashboard ready |
| Iterate based on feedback | Dev | 1 week | Test results |

**Deliverables:**
- [ ] Rip and Ship Dashboard live
- [ ] OBS overlay working
- [ ] Pull logging for dispute protection
- [ ] Post-stream export to shipping

---

### Phase 4: Inventory System (Week 11-16)

**Goal:** Organize physical inventory

| Task | Owner | Time | Dependencies |
|------|-------|------|--------------|
| Build intake workflow | Dev | 2 weeks | Hardware ready |
| Build zone tracking | Dev | 1 week | Intake workflow |
| Integrate with Shopify POS | Dev | 1-2 weeks | Zone tracking |
| Initial inventory cataloging | Team | 2-4 weeks | System ready |
| Staff training | Team | 1 week | System ready |

**Deliverables:**
- [ ] Inventory system live
- [ ] Barcode labels on items
- [ ] Staff can find any item quickly
- [ ] Sales automatically tracked

---

### Phase 5: Advanced Tools (Week 17+)

**Goal:** Specialized optimizations

| Task | Owner | Time | Dependencies |
|------|-------|------|--------------|
| Card Show Prep Tool | Dev | 3-4 weeks | Inventory system |
| Lot Cost Tracker | Dev | 3-4 weeks | Inventory system |
| Roca Output Analytics | Dev | 2-3 weeks | Lot tracker |
| Customer-facing kiosk | Dev | 2 weeks | Inventory system |

**Deliverables:**
- [ ] Card show prep automated
- [ ] Lot profitability tracked
- [ ] Roca runs analyzed
- [ ] Customers can browse digitally

---

## Timeline Visualization

```
Jan 2026
├── Week 1-2: Price Lookup App + Batch Shipping
│
Feb 2026
├── Week 3-4: eBay + Liquidity integration
├── Week 5-6: Tax automation + Zone planning
│
Mar 2026
├── Week 7-8: Rip and Ship Dashboard build
├── Week 9-10: Dashboard testing + iteration
│
Apr-May 2026
├── Week 11-14: Inventory System build
├── Week 15-16: Initial cataloging + training
│
Jun 2026+
├── Week 17+: Card Show Tool, Lot Tracker, Analytics
```

---

## Quick Reference: What to Build First

### If you have 1 week:
→ **Price Lookup App** (Phase 1 MVP)

### If you have 2 weeks:
→ Price Lookup App + **Batch Shipping**

### If you have 1 month:
→ Price Lookup + Batch Shipping + **Liquidity scoring** + Tax automation

### If you have 3 months:
→ All above + **Rip and Ship Dashboard** + **Inventory System foundation**

---

## Decision Points

### After Phase 1:
- Is Price Lookup providing value?
- Which device(s) work best?
- Ready to add liquidity scoring?

### After Phase 2:
- Is tax automation working smoothly?
- Ready for inventory system?
- Hardware procured?

### After Phase 3:
- Is Rip and Ship Dashboard improving streams?
- What features are missing?
- Ready for inventory cataloging effort?

### After Phase 4:
- Is inventory system being used consistently?
- What's falling through the cracks?
- Which advanced tool is most needed?

---

## Dependencies Map

```
                    ┌─────────────────┐
                    │  TCGplayer API  │
                    │    Access       │
                    └────────┬────────┘
                             │
                             ▼
┌─────────────────┐  ┌─────────────────┐
│  eBay API       │  │  Price Lookup   │
│  Access         │  │  App            │
└────────┬────────┘  └────────┬────────┘
         │                    │
         └────────┬───────────┘
                  │
                  ▼
         ┌─────────────────┐
         │  Liquidity      │
         │  Assessment     │
         └────────┬────────┘
                  │
    ┌─────────────┼─────────────┐
    ▼             ▼             ▼
┌────────┐  ┌──────────┐  ┌──────────┐
│Inventory│ │Card Show │  │Buy/Trade │
│System   │ │Prep Tool │  │Workflow  │
└────────┘  └──────────┘  └──────────┘
```

---

## Risk Factors

| Risk | Mitigation |
|------|------------|
| API access delays | Start application process early |
| Staff adoption | Involve team in design, make it simple |
| Time constraints | Prioritize ruthlessly, ship MVPs |
| Scope creep | Stick to defined phases |
| Technical debt | Build clean from the start |

---

## Success Metrics

### Phase 1
- [ ] Price lookups < 5 seconds
- [ ] Staff using app daily
- [ ] Post-stream shipping time cut by 50%+

### Phase 2
- [ ] Liquidity check on every buy decision
- [ ] Sales tax automated (no manual filing)
- [ ] Staff confident quoting prices

### Phase 3
- [ ] Streams run smoother
- [ ] No order tracking errors
- [ ] Disputes have pull log evidence

### Phase 4
- [ ] Any item findable in < 30 seconds
- [ ] Sales automatically tracked
- [ ] Stock levels accurate

---

Back to [[JinkiesCo - Project Index]]
