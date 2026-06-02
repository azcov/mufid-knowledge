---
tags: [requirements, architecture]
---

# Technical Architecture — BudgetNest

## Stack
| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14+ (App Router), Radix UI |
| Mobile | React Native (Expo) — post-MVP |
| Backend | Go + Gin + Sentry |
| Database | PostgreSQL via Supabase (Singapore) |
| Auth | Supabase Auth (email + Google OAuth) |
| Storage | Supabase Storage (receipt images) |
| Payments | Xendit (QRIS, VA, GoPay, OVO, DANA) |
| AI | Anthropic (receipt scan) |
| Email | Resend |
| Frontend host | Vercel |
| Backend host | Railway / Oracle VM + Caddy |
| Observability | Sentry + OpenTelemetry |

## Architecture Pattern
`handler → usecase → repository`
- Handlers: HTTP concerns only
- Usecases: business logic, tier enforcement
- Repositories: GORM, return domain types

## API Design
RPC-style — all mutations POST, no PUT/PATCH/DELETE, no path params.

| Action | Method | Pattern |
|--------|--------|---------|
| Read one | GET | `/api/v1/{resource}?id=uuid` |
| List | POST | `/api/v1/{resource}/list` |
| Create | POST | `/api/v1/{resource}` |
| Update | POST | `/api/v1/{resource}/update` |
| Soft delete | POST | `/api/v1/{resource}/delete` |

Admin endpoints: `/api/v1/admin/*`

## Auth Flow
1. User signs in via Supabase Auth → JWT
2. Frontend: `Authorization: Bearer <token>`
3. Go middleware validates JWT (HS256 with `SUPABASE_JWT_SECRET`)
4. First login: Supabase webhook → Go creates `users` row

## Database Conventions
- No FK constraints — enforced at application level
- TEXT not VARCHAR
- UUID PKs via `uuid_generate_v4()`
- Soft deletes: `deleted_at TIMESTAMPTZ`
- All timestamps UTC
- Migrations: `golang-migrate` SQL files only

## Key Entities
- `users` — Supabase auth mirror
- `households` — tenant unit (up to 4 members)
- `household_members` — user ↔ household join
- `subscriptions` — Xendit subscription state
- `categories` — per-household (system + custom)
- `accounts` — bank/e-wallet tags per household
- `expenses` — core transaction log
- `budget_plans` — monthly targets per category
