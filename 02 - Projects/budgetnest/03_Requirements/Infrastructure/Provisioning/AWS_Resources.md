---
tags: [provisioning, infrastructure]
---

# Provisioning — BudgetNest

## Hosting Strategy
| Env | Backend | Frontend | DB |
|-----|---------|----------|----|
| Dev | Docker Compose (local) | `npm run dev` | Docker PostgreSQL |
| Staging | Railway | Vercel Preview (per PR) | Supabase staging project |
| Prod | Oracle VM + Caddy | Vercel | Supabase production project |

## Local Dev (docker-compose.yml)
```yaml
services:
  postgres: PostgreSQL 16 → port 5432
  migrate: golang-migrate (runs on startup)
  api: Go backend → port 8080
  web: Next.js → port 3000
```

## Production (docker-compose.prod.yml)
```yaml
services:
  caddy: Caddy 2 reverse proxy (ports 80/443/HTTP3, auto-TLS)
  api: Go API from GHCR image → port 8080 (internal)
```
- Image: `ghcr.io/{owner}/budgetnest-api:{tag}`
- Env file: `/opt/budgetnest/.env` on the server

## CI/CD (GitHub Actions)
- Trigger: push to `backend/**`
- Steps: `make test` → `make lint` → build → push to GHCR
- Frontend: Vercel auto-deploy on push to `main`

## Railway Config
```toml
[deploy]
startCommand = "./bin/api"
healthcheckPath = "/health"
healthcheckTimeout = 60
restartPolicyType = "on-failure"
```

## Supabase Setup
- Region: Singapore (ap-southeast-1) — lowest latency for Indonesian users
- Auth: email + Google OAuth
- Storage bucket: `receipts` (private)
