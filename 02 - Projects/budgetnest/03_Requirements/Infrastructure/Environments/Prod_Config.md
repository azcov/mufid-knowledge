---
tags: [environment, prod]
---

# Production Environment — BudgetNest

## Hosting
| Service | Platform |
|---------|----------|
| Backend | Oracle VM + Caddy reverse proxy |
| Frontend | Vercel (auto-deploy from `main`) |
| Database | Supabase production project (Singapore) |
| Images | GHCR — `ghcr.io/{owner}/budgetnest-api:{tag}` |

## Deploy
```bash
# Backend (on Oracle VM)
docker compose -f docker-compose.prod.yml pull
docker compose -f docker-compose.prod.yml up -d
```

## Pre-deploy Checklist
1. `make test` passes
2. `make lint` clean
3. `make build` succeeds
4. `make migrate-up` on prod DB
5. All env vars set on server
6. `GET /health` returns 200

## Rollback
1. Backend: `docker compose -f docker-compose.prod.yml up -d --no-build` with previous image tag
2. Frontend: Vercel dashboard → instant rollback
3. DB: `make migrate-down` (only if new migration broken — have `down.sql` ready)

## Monitoring
- Sentry: set `SENTRY_DSN` for error tracking
- OpenTelemetry: set `OTEL_EXPORTER_OTLP_ENDPOINT` for traces
- Caddy health check: auto-restarts on failure
