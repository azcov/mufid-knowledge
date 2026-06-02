---
tags: [management, roadmap]
---

# Roadmap — Bengkelterdekat

## Done ✅ (from sessions ~2026-05-31)
- Backend Go (chi + pgx) — API for workshops, tags, cities
- Frontend Next.js with Base UI + React Query
- SEO metadata table added to DB
- Facility module implemented
- SEO modules (JSON-LD, sitemap support)
- Tag price range support
- Committed and pushed to GitHub

## In Progress 🔧
- Filling out remaining backend modules (owner claim, subscriptions, analytics events)
- Frontend public directory pages (search, listing card, detail page)

## Planned 📅

### Era 1 (Months 1–6): Foundation & Revenue
**Phase 001** — DB schema + admin panel + CSV import (workshops, tags, cities, owners, subscriptions)
**Phase 002** — Public directory (search + filters + workshop detail pages, SSR, mobile-first, AdSense)
**Phase 003** — SEO landing pages `/bengkel/[tag]/[city]` — 140+ pages (programmatic)
**Phase 004** — Owner claim flow + Xendit payment + owner dashboard
**Phase 005** — 500+ listing seeding across 10 cities + launch
**Phase 006** — Direct sales to workshop owners, unit economics gate

**Gate:** 200 paying subscribers, 50K sessions/mo, MRR Rp 29.8M, churn < 5%

### Era 2 (Months 7–12): Growth
- Motor listings
- 10 more cities
- Visitor review system
- Premium placement auction

### Era 3 (Months 13–18): Platform
- Analytics Pro, B2B spare parts partnership, React Native mobile app

## Recent Commits
- Added `seo_metadata` table for structured SEO per listing
- Added facility and SEO modules to backend
- Tag price range support (Budget/Mid-range/Premium)
