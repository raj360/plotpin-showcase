# PlotPin

Public showcase for PlotPin, a map-first rental discovery product (Uganda launch market).

Tenants browse buildings on a map. Listings stay approximate until they pay to unlock verified landlord contact and the exact pin. Landlords list for free after admin verification.

## Problem statement

In markets like Uganda, rental search is full of friction. Tenants pay “blockers” for tours, meet intermediaries instead of landlords, and hit bait pricing. Landlords get low-quality leads. Trust is hard to verify on classified-style apps.

## Solution path

1. **Map-first discovery:** show supply where people already search geographically.
2. **Free verified listings:** landlords submit buildings and units, admins verify, then units can go available.
3. **Approximate until unlock:** protect exact pins and contact until a tenant pays.
4. **Paid unlock:** tenant unlocks contact (and exact location) for a timed exclusive window on that unit.
5. **Serious-inquiry economics:** unlock fees and optional featured boosts replace mandatory listing fees.
6. **Payments that fit the market:** local rails (for example Flutterwave / MoMo markets) plus international card checkout for diaspora.

## Tech stack

| Area | Choices |
| --- | --- |
| Web | Next.js, TypeScript, Google Maps |
| API | NestJS REST |
| Data | Supabase Postgres, PostGIS |
| Auth | Supabase auth patterns |
| Payments | Flutterwave, Lemon Squeezy |
| Monorepo | Yarn workspaces, Turborepo |
| Ops | Docker Compose for optional local PostGIS |

## Architecture (clean plan)

```
┌──────────────────┐     ┌──────────────────┐
│  Web (Next.js)   │────▶│  API (NestJS)    │
│  explore, unlock │     │  buildings,      │
│  landlord, admin │     │  unlocks, pay    │
└────────┬─────────┘     └────────┬─────────┘
         │                        │
         │               ┌────────▼─────────┐
         │               │ Supabase Postgres│
         │               │ + PostGIS        │
         │               └────────┬─────────┘
         │                        │
         │         ┌──────────────┼──────────────┐
         │         ▼              ▼              ▼
         │   pricing_rules   unlocks/wallet   verification
         │   featured        payments         media
         ▼
   Maps + FX display (viewer currency hints)
```

**Bounded contexts**
- **Supply:** buildings, units, photos, listing status
- **Trust:** admin verification, anti-scam rules
- **Discover:** map bounds, featured ranking, approximate location
- **Unlock:** quote, pay, exclusive contact window
- **Grow:** coupons, credits, featured boosts

**Role on this project:** product and engineering ownership across API, web, monetization direction, and Uganda launch flows.

## Note

This repo is a README showcase. Production source is maintained separately.
