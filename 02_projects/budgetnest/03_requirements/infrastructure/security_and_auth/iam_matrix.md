---
tags: [security, auth, iam]
---

# Security & Auth — BudgetNest

## Auth Strategy
- Provider: Supabase Auth (email/password + Google OAuth)
- Token: JWT HS256, signed by `SUPABASE_JWT_SECRET`
- Transport: `Authorization: Bearer <token>` header
- Claims: `user_id`, `app_role`, `email`, `exp`

## IAM Roles
| Role | Access | Verified by |
|------|--------|-------------|
| `user` | Own household data only | JWT + `household_id` filter |
| `admin` | All data via `/api/v1/admin/*` | `app_role = "admin"` in JWT |
| Xendit webhook | Payment events only | `XENDIT_WEBHOOK_TOKEN` constant-time compare |
| Supabase webhook | User creation events | `SUPABASE_WEBHOOK_SECRET` |

## Authorization Rules
- All DB queries must filter by `household_id` — never expose other households
- Freemium limits enforced in usecase layer, not frontend
- Admin endpoints under `/api/v1/admin/` — middleware checks role
- No raw SQL — GORM parameterized queries only
- Soft deletes only (`deleted_at`) — never hard delete

## Secrets
| Secret | Where stored |
|--------|-------------|
| `SUPABASE_JWT_SECRET` | Railway env / server .env |
| `SUPABASE_ANON_KEY` | Vercel env (public ok) |
| `XENDIT_SECRET_KEY` | Railway env |
| `XENDIT_WEBHOOK_TOKEN` | Railway env |
| `ANTHROPIC_API_KEY` | Railway env |
| `RESEND_API_KEY` | Railway env |
| `SENTRY_DSN` | Railway env |

## API Security
- Rate limiting per user/IP on public endpoints
- CORS: `ALLOWED_ORIGINS` — explicit, no wildcard
- Input validation: go-playground/validator at handler boundary
- Request size limit on upload endpoints
