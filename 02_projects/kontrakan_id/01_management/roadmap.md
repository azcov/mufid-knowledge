---
tags: [management, roadmap]
---

# Roadmap — Kontrakan / Kostify

## Done ✅ (from sessions ~2026-05-31)
- Product requirements: full documentation written in `/projects/kostify/docs/`
- Architecture designed: Go (chi) + Next.js + React Native (Expo)
- Auth decision: **OTP only** (WhatsApp OTP + phone OTP + email OTP + Google OAuth) — no password
- Xendit subscription billing integrated
- Documentation written for all API endpoints (properties, units, tenants, contracts, payments, reminders, dashboard)
- Repo: single monorepo at `/projects/kostify/` (backend + web + mobile)

## Business Decisions (from session)
- **No booking feature** — landlord-only MVP, tenants don't self-book
- **Auto reminders via Fonnte** (Pro tier) — WhatsApp gateway, no official WA Business API needed
- **Payment tracking only** — no online payment collection (tracks rent, doesn't process it)
- Higher plan = auto WA gateway via Fonnte

## In Progress 🔧
- Backend Go API (chi router) implementation
- React Native mobile (Expo) — iOS + Android
- Next.js web app

## Planned 📅

### MVP (Now → Month 3)
- Auth: phone/email OTP + Google OAuth (no passwords)
- Property + unit management
- Tenant profiles + contracts
- Auto payment cycle generation
- Manual payment logging
- Dashboard (income, occupancy, late count)
- Monthly income report (expected vs actual)
- WhatsApp reminders (manual wa.me + Fonnte auto for Pro)
- Contract expiry alerts
- Xendit billing (Free → Basic → Pro)

### v2 (Month 3–6)
- Payment receipt upload
- Tenant document storage
- Maintenance request log
- Annual income summary (PPh reporting)
- Referral program

### v3 (Month 6–12)
- Shareable vacant unit page (tenant self-apply)
- Multi-user (owner + penjaga kos)
- Bulk WhatsApp blast
- Xendit QRIS per-unit (auto-reconcile)

## Infrastructure
- Backend: Railway (Go, chi router)
- Web: Vercel (Next.js)
- Mobile: Expo + App Store + Play Store
- DB: PostgreSQL (Supabase, Singapore)
- Auth: Supabase (OTP + Google OAuth)
- WA Gateway: Fonnte (Pro tier)
- Billing: Xendit
- Repo: pnpm workspace monorepo
