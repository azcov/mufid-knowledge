---
tags: [management, roadmap]
---

# Roadmap — DeenDaily

## Done ✅
- Backend: Go + Gin, DDD pattern
- Frontend: React + Radix UI
- Mobile: React Native + Expo structure

## In Progress 🔧 (from 2026-06-02 session)
**Major architecture migration:**
- Auth: migrating to **Neon Auth** (JWKS-based JWT from Neon)
- DB: migrating to **Neon** (PostgreSQL with auto-branching)
- Mobile paywall: **Superwall** (iOS + Android in-app purchase paywall management)
- Web payments: **Stripe** (subscription management)
- Wrote full plan docs in `docs/` before implementing
- Goal: production-grade security, infra, code

## Planned 📅

### Phase 1 MVP: Q3 2026
- Daily check-in screen (salah + sunnah + custom habits)
- Streak counter + 7-day history (free) / full history (paid)
- Push notifications (daily reminder)
- IAP via App Store + Play Store (server-side receipt validation via Stripe)
- English + Bahasa Indonesia
- iOS + Android on Expo
- Backend on Render.com (Singapore)

### Phase 2: Retention Q4 2026
- Analytics, monthly insights (paid)
- Shareable summary image
- Streak freeze (paid)
- Annual subscription

### Phase 3: Community Q1–Q2 2027
- Halaqah groups (3–8 person accountability circles)
- Group streaks + leaderboard

## Infrastructure Stack (current)
| Service | Choice |
|---------|--------|
| Backend host | Render.com Singapore (starter) |
| DB | Neon (PostgreSQL, auto-pause dev) |
| Auth | Neon Auth (JWKS JWT) |
| Mobile paywall | Superwall |
| Web payments | Stripe |
| Build | Docker + render.yaml |
