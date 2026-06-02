---
tags: [environment, dev]
---

# Dev Environment — BudgetNest

## Setup
```bash
cp .env.example .env  # fill SUPABASE_* vars
docker compose up -d  # starts postgres + migrate + api + web
```

## Services
| Service | Port |
|---------|------|
| PostgreSQL | 5432 (db: budgetnest, user: postgres, pw: postgres) |
| Go API | 8080 |
| Next.js | 3000 |

## Env Vars
| Var | Required | Default/Notes |
|-----|----------|---------------|
| `SUPABASE_JWT_SECRET` | ✅ | Supabase → Settings → API → JWT Secret |
| `SUPABASE_URL` | ✅ | Your Supabase project URL |
| `SUPABASE_ANON_KEY` | ✅ | Supabase public anon key |
| `SUPABASE_WEBHOOK_SECRET` | optional | `local-webhook-secret` |
| `XENDIT_SECRET_KEY` | optional | blank = skip payments |
| `XENDIT_WEBHOOK_TOKEN` | optional | `local-xendit-token` |
| `ANTHROPIC_API_KEY` | optional | blank = skip receipt scan |
| `RESEND_API_KEY` | optional | blank = skip email |
| `APP_URL` | optional | `http://localhost:3000` |
| `SENTRY_DSN` | optional | blank locally |
| `ALLOWED_ORIGINS` | set | `http://localhost:3000` |

## Health Check
```bash
curl http://localhost:8080/health  # should return 200
```
