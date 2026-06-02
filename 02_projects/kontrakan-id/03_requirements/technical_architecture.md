---
tags: [requirements, architecture]
---
# Technical Architecture — Kontrakan / Kostify

## Stack (from kostify codebase)
| Layer | Technology |
|-------|-----------|
| Backend | Go + chi router |
| Mobile | React Native (Expo) — iOS + Android |
| Web | Next.js |
| Database | PostgreSQL via Supabase (Singapore) |
| Auth | Supabase Auth (phone OTP + email OTP + Google OAuth) |
| Payments | Xendit (subscription billing) |
| WA Gateway | Fonnte (automated reminders, Pro tier) |
| Storage | Supabase Storage (tenant documents, Pro) |
| Host | Railway (backend) + Vercel (web) |
| Build tool | pnpm workspace (monorepo) |

## Architecture
```
Mobile App (Expo) + Web (Next.js)
    │ Bearer JWT (Supabase)
    ▼
Go API (Chi router, port 8080)
    ├── Background Scheduler (goroutine, 1hr tick)
    │   ├── Mark late payment cycles
    │   ├── Send reminders via Fonnte (Pro)
    │   └── Send contract expiry alerts
    ├── PostgreSQL (Supabase)
    ├── Supabase Auth
    ├── Fonnte (WA gateway)
    └── Xendit (billing webhooks)
```

## Scheduler Note
In-process goroutine (fine for MVP up to ~10K cycles).
Phase 2: migrate to pg_cron or Inngest for observability + retries.

## Key Entities
- `properties`, `units`, `tenants`, `contracts`
- `payment_cycles` — auto-generated per contract
- `whatsapp_logs` — reminder delivery tracking
- `subscriptions` — Xendit subscription state
