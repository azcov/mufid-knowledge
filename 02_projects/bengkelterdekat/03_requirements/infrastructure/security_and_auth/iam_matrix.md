---
tags: [security, auth, iam]
---

# Security & Auth — Bengkelterdekat

## Auth Strategy
- Visitors: no auth required (public directory)
- Workshop owners: Supabase Auth (email + JWT)
- Admins: Supabase Auth with `app_role = "admin"`

## IAM Roles
| Role | Access |
|------|--------|
| Anonymous visitor | Public directory, detail pages only |
| Workshop owner | Own listing edit, own analytics, own subscription |
| Admin | All listings, claim queue, all data via `/api/v1/admin/*` |
| Xendit webhook | Payment events only, validated by `WEBHOOK_SECRET` |

## Key Security Rules
- Visitors never need to create an account — zero friction for car owners
- Owner can only edit their own claimed workshop
- Xendit webhook: constant-time comparison against `XENDIT_WEBHOOK_TOKEN`
- No raw SQL — pgx parameterized queries only
- Soft deletes only

## Secrets
| Secret | Storage |
|--------|---------|
| `SUPABASE_JWT_SECRET` | Railway env |
| `XENDIT_SECRET_KEY` | Railway env |
| `XENDIT_WEBHOOK_TOKEN` | Railway env |
| `SUPABASE_URL` / `SUPABASE_ANON_KEY` | Vercel env |
