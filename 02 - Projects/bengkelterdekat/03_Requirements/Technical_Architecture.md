---
tags: [requirements, architecture]
---

# Technical Architecture — Bengkelterdekat

## Stack
| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14+ (App Router), Base UI + React Query + Supabase client |
| Mobile | React Native (Expo) — Era 3 |
| Backend | Go + chi router + pgx (no ORM) |
| Database | PostgreSQL via Supabase (Singapore) |
| Auth | Supabase Auth (email + JWT) |
| Storage | Supabase Storage (workshop photos) |
| Payments | Xendit |
| SEO | Next.js SSR + sitemap + JSON-LD + Open Graph |
| Ads | Google AdSense |
| Frontend host | Vercel |
| Backend host | Railway / AWS ECS |
| Design system | `/design-system` folder in monorepo |

## Monorepo Structure
```
bengkelterdekat/
├── backend/       Go API (chi, pgx)
├── frontend/      Next.js (Base UI, React Query, Supabase)
├── mobile/        React Native (Expo) — future
├── design-system/ Shared design tokens
└── docs/          Product, architecture, engineering docs
```

## Architecture Pattern
`handler → usecase → repository`
- chi router (not Gin — explicit in go.mod)
- pgx/v5 directly (no GORM) — performance-sensitive for directory queries
- JWT: Supabase HS256 validation in middleware

## Key DB Entities
- `workshops` — core listing (name, city, address, phone, price_range, vehicle_type)
- `specialization_tags` — taxonomy (kaki-kaki, AC, transmisi, body repair, dll)
- `workshop_tags` — many-to-many join
- `cities` — 10 initial cities
- `owners` — claimed workshop owners
- `subscriptions` — Xendit subscription state
- `analytics_events` — view/phone/WhatsApp tap events

## SEO Architecture
- SSR via Next.js App Router — all listing pages server-rendered
- Programmatic pages: generated from DB at build/request time
- ISR for frequently updated pages (listing edits)
- JSON-LD LocalBusiness schema on all detail pages
