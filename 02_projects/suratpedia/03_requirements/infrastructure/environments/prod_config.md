---
tags: [environment, prod]
---

# Production Environment

## URL

- Frontend: https://suratpedia.com
- Backend: not publicly documented (behind frontend or a separate subdomain)
- R2 public files: https://r2.suratpedia.com

## Env Vars

### Frontend
| Variable | Production value | Notes |
|----------|-----------------|-------|
| `NEXT_PUBLIC_API_URL` | `https://<backend-domain>` | Must be the public backend URL, not `http://backend:8080` (Docker service name only works inside Docker network) |
| `REVALIDATE_SECRET` | Long random string | Must match backend value |

### Backend
| Variable | Production value | Notes |
|----------|-----------------|-------|
| `DATABASE_URL` | `postgres://...@db:5432/suratpedia?sslmode=disable` | Internal Docker network if co-hosted |
| `REDIS_URL` | `redis:6379` | Internal Docker network |
| `R2_ACCOUNT_ID` | Cloudflare Account ID | Real production credentials |
| `R2_ACCESS_KEY` | Real R2 Access Key | Never commit to git |
| `R2_SECRET_KEY` | Real R2 Secret Key | Never commit to git |
| `R2_BUCKET` | `suratpedia-storage` | Production bucket |
| `R2_PUBLIC_URL` | `https://r2.suratpedia.com` | Custom domain for public file access |
| `PARSER_URL` | `http://parser:8001` | Internal Docker network |
| `JWT_SECRET` | Long random string (min 32 chars) | Never commit to git |
| `ADMIN_EMAIL` | Real admin email | Stored in env only |
| `ADMIN_PASSWORD` | Strong password | Never commit to git |
| `ENV` | `production` | Enables `Secure` flag on JWT cookie |
| `REVALIDATE_SECRET` | Same long random string as frontend | Must match |

## Deploy Process

```bash
# On the VPS, from the repo directory:
git pull origin main

# Rebuild changed services
docker-compose up -d --build

# Or rebuild a specific service
docker-compose up -d --build backend
docker-compose up -d --build frontend
```

**Startup order** is enforced by Docker Compose `depends_on` with healthchecks:
1. PostgreSQL becomes healthy (`pg_isready`, up to 10 retries × 5s)
2. Redis starts
3. Backend starts (after both db and redis are ready)
4. Frontend starts (after backend is ready)

## Production Checklist

From the deployment docs — items to complete before going live:

- [ ] Replace all default secrets in `.env` (JWT_SECRET, ADMIN_PASSWORD, REVALIDATE_SECRET)
- [ ] Set `ENV=production` in backend for `Secure` cookie flag
- [ ] Configure CORS in backend to allow only `suratpedia.com` (not wildcard)
- [ ] Set up Cloudflare custom domain `r2.suratpedia.com` → R2 bucket
- [ ] Set up Cloudflare proxy/CDN for frontend (optional but recommended)
- [ ] Set `NEXT_PUBLIC_API_URL` to the real backend URL (not Docker service name)
- [ ] Add PostgreSQL indexes (see database schema docs)
- [ ] Ensure PostgreSQL named volume is backed up on a schedule
- [ ] Monitor R2 disk usage — temp files accumulate if cron is not running
- [ ] Verify cron job starts (logged as `[cron] Starting cleanup job` at 03:00 daily)
- [ ] Run `go mod tidy` and rebuild backend after any `go.mod` changes

## Monitoring / Alerts

No formal monitoring stack is documented (no Prometheus, Grafana, Datadog, etc.).

Manual monitoring commands:
```bash
# Check all services are running
docker-compose ps

# Tail backend logs (includes cron output)
docker-compose logs -f backend

# Check R2 temp file accumulation
# (via Cloudflare R2 dashboard — bucket size, object count by prefix)

# Check Redis cache health
docker-compose exec redis redis-cli PING
docker-compose exec redis redis-cli DBSIZE
```

Cron log format to watch for:
```
[cron] Starting cleanup job
[cron] Deleted N short-tier expired files
[cron] Downgraded N medium-tier inactive files
[cron] Upgraded N template to permanent
[cron] Cleanup done
```
