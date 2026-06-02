---
tags: [management, roadmap]
---

# Roadmap — BudgetNest

## Done ✅ (from sessions ~2026-05-31)
- Backend: Go + Gin, full folder structure (cmd/api, internal/config, handlers, usecases, repos)
- Resend integration (email)
- Sentry integration (error tracking)
- OpenTelemetry setup (tracing)
- Railway config for Go deployment
- Docker Compose (postgres + migrate + api + web)
- Security review complete
- Production infra: Oracle VM + Caddy + GHCR Docker image

## In Progress 🔧
- Working toward full production-ready release (web + mobile, Indonesian market)
- Mobile: React Native / Expo structure in place

## Planned 📅

### Phase 1 MVP: Q2–Q3 2026
- Manual expense logging (< 5s)
- Budget categories (10 Bahasa Indonesia defaults + custom, free: 5)
- Monthly budget plan per category
- Monthly summary report
- AI receipt scan (5/mo free, unlimited paid) — Anthropic API
- Bank/e-wallet account tagging
- Shared household ledger up to 4 (paid)
- Household invite flow
- Xendit billing (QRIS, VA, GoPay, OVO, DANA)
- Freemium gating server-side

### Phase 2: Retention Q3–Q4 2026
- Trend reports, budget alerts, recurring templates
- CSV export, full history

### Phase 3: Intelligence Q1 2027
- AI categorization, smart savings targets
- React Native mobile app
- Indonesian bank auto-sync (post-PMF)

## Infrastructure
| Service | Choice |
|---------|--------|
| Backend host | Oracle VM (free tier) + Caddy reverse proxy |
| Frontend | Vercel (auto-deploy from main) |
| DB | Supabase PostgreSQL (Singapore) |
| Auth | Supabase Auth |
| CI/CD | GitHub Actions → GHCR → Oracle VM |
| Payments | Xendit |
| Email | Resend |
| Errors | Sentry |
| Traces | OpenTelemetry |
