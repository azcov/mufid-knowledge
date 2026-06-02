---
tags: [requirements, architecture]
---
# Technical Architecture — IndoDev-ID

## Stack
| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14+ (App Router), Base UI + React Query + Supabase |
| Backend | Go + Gin + pgx |
| Database | PostgreSQL via Supabase (Singapore) |
| Auth | Supabase Auth (email + Google OAuth) |
| Payments | Xendit (QRIS, VA, GoPay, OVO, DANA) |
| Frontend host | Vercel |
| Backend host | Railway |

## Architecture Pattern
`handler → usecase → repository` (same as all labs projects)

## Key Entities
- `users` — Supabase mirror (developers + company users)
- `developer_profiles` — stack, seniority, location, open-to-remote
- `companies` — name, logo, size, industry
- `job_posts` — role, salary_min, salary_max (required), stack, description
- `applications` — developer × job with status
- `boosts` — featured/email blast/candidate search per company, Xendit-backed
