---
tags: [management, roadmap]
---

# Roadmap — Kulitku

## Done ✅ (from sessions)
- Backend: Go + Gin, handler→usecase→repository pattern
- Frontend: React + Base UI + React Query + Supabase + barcode scan (ZXing)
- 100 manually curated MVP products
- BPOM number and halal cert tracking
- Supabase Auth (email + Google)
- Gamification / points economy (implemented ~2026-05-31)
- Sociolla scraper using Go Colly (from sitemap)
- Docker Compose local dev setup

## In Progress 🔧
- Mobile app — React Native + Expo (iOS + Android, production-ready goal)
  - Includes all features from web: ingredient checker, subscriptions, etc.

## Planned 📅

### Phase 2: Scale & Trust
- Crowdsourced content (user-submitted products/ingredients)
- AI OCR for ingredient label scanning (camera → extract INCI list)
- Halal certification deadline tracker (BPJPH October 2026)
- Ingredient alias lookup + INCI parsing service

### Phase 3: Global (Dermiq brand)
- Dual-brand architecture: Kulitku (ID) + Dermiq (global)
- Same backend, separate frontends, feature flags
- Global INCI database expansion

## Never
- Full e-commerce (sell products) — ingredient checker only
- Doctor consultation platform
- Social/community feed (Phase 4 at earliest)

## Key Technical Notes
- Barcode scanning: ZXing browser library in frontend (scan product in-store)
- Scraper: Go Colly scraping Sociolla via sitemap (product data enrichment)
- Gamification: point economy for crowdsourced contributions
- Repo: `/Users/mufid/projects/kulitku/` — monorepo (backend + frontend + mobile + scraper)
