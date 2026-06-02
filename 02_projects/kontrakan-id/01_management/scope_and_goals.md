---
tags: [management, scope]
---
# Scope & Goals — Kontrakan (Kostify)

## Goal
Property management for Indonesian small landlords (1–20 units). Track payments, send automated WhatsApp reminders, manage contract expiry, view income health. Replaces WhatsApp groups and paper notebooks.

## Two Repos
- `kontrakan-id/` — product docs + planning
- `kostify/` — actual code (backend Go, mobile React Native, pnpm workspace)

## In Scope (MVP)
- Property + unit management (tiered limits)
- Tenant profiles + contract management
- Auto payment cycle generation from contract
- Manual payment logging
- Dashboard: income summary, occupancy rate, late count
- Monthly income report (expected vs actual, per unit)
- WhatsApp reminders:
  - Manual: wa.me deep link (all tiers)
  - Automated: Fonnte gateway (Pro only)
- Contract expiry alerts (Basic + Pro)
- Xendit subscription billing
- Next.js web app + React Native mobile app (Expo)
- Auth: phone OTP + email OTP + Google OAuth (passwordless)

## Out of Scope (MVP)
- Online rent collection / payment gateway (tracks payment, doesn't process it)
- Tenant-facing app / tenant portal (Phase 2)
- Property listing marketplace (different product — Mamikos)
- Legal document generation (kontrak sewa)
- Maintenance vendor marketplace
- Tax filing / PPh calculation (shows data only)
- Large property management (> 30 units, > 3 properties) — not v1

## Pricing
| Tier | Price | Units |
|------|-------|-------|
| Free | Rp 0 | Up to 3 units, manual logging only, no reminders |
| Basic | Rp 49.000/mo | Up to 10 units, auto WA reminders, contract alerts, history |
| Pro | Rp 99.000/mo | Up to 30 units, multi-property, full income report, document storage |

## Success Metrics (Month 6)
| Metric | Target |
|--------|--------|
| Paying landlords | 300 |
| MRR | ≥ Rp 18M |
| Free-to-paid conversion | ≥ 20% |
| Units tracked | 2,000+ |
| Monthly churn | ≤ 4% |
| WA reminders sent/month | 5,000+ |
