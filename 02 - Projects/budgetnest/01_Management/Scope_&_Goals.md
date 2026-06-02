---
tags: [management, scope]
---

# Scope & Goals — BudgetNest

## Goal
Indonesian household budget tracker — fast expense logging, category budgets, and a shared ledger both partners see in real time. Freemium B2C, Rp 29K/month paid tier.

## In Scope (MVP)
- Manual expense logging (< 5 seconds per entry)
- Budget categories (10 system defaults in Bahasa Indonesia + custom; free tier capped at 5)
- Monthly budget plan per category (actual vs plan, real-time)
- AI receipt scan (5/month free, unlimited paid) — Anthropic vision API
- Bank/e-wallet account tagging (manual, no sync; free: 1 account, paid: unlimited)
- Shared household ledger up to 4 members — paid only
- Household invite flow (email or link)
- Auth: email + Google OAuth (Supabase Auth)
- Xendit payment (QRIS, bank transfer, GoPay, OVO, DANA)
- Full Bahasa Indonesia UI
- Mobile-first web (Next.js responsive)

## Out of Scope (MVP)
- Bank auto-sync / Open Banking
- Financial product marketplace (loans, insurance, investment)
- Investment / portfolio tracking
- AI spending predictions or coaching
- React Native mobile app — post-PMF
- Multi-currency (IDR only)
- PDF/accounting exports

## Pricing
| Tier | Price | Limits |
|------|-------|--------|
| Free | Rp 0 | 1 user, 5 AI scans/mo, 5 categories, 1 account, current month only |
| Paid | Rp 29.000/mo · Rp 290.000/yr | Up to 4 members, unlimited everything, full history, trends, CSV export |

## Success Metrics (Month 6)
| Metric | Target |
|--------|--------|
| Paid conversion | ≥ 5% of MAU |
| D30 retention | ≥ 35% |
| WAU/MAU | ≥ 0.4 |
| Activation (≥5 entries week 1) | ≥ 50% of signups |
| Household invite rate | ≥ 60% of paid users |
| Paying households | 300 |
| MRR | Rp 8.7M |
