---
tags: [environment, dev]
---

# Dev Environment

## Setup

Prerequisites: Docker, Docker Compose, Node.js (for local frontend dev if needed).

```bash
# Clone and start all services
git clone <repo>
cd suratpedia
docker-compose up -d

# Check all services are healthy
docker-compose ps

# Tail logs
docker-compose logs -f backend
docker-compose logs -f frontend
docker-compose logs -f parser
```

On first start, PostgreSQL runs `db/init.sql` automatically (Docker entrypoint convention). This creates all tables, seeds 8 categories, seeds 3 SEO metadata entries, and inserts 2 sample templates.

Access points after startup:

| Endpoint | URL |
|----------|-----|
| Frontend | http://localhost:3000 |
| Admin panel | http://localhost:3000/admin |
| Backend API | http://localhost:8080 |
| API health | http://localhost:8080/api/categories |
| Parser health | http://localhost:8001/health |
| PostgreSQL | localhost:5432 |
| Redis | localhost:6379 |

## Env Vars

Copy `.env.example` to `.env` and fill in values. In Docker Compose, env vars are injected per service.

### Frontend (`frontend` service)

| Variable | Dev value | Description |
|----------|-----------|-------------|
| `NEXT_PUBLIC_API_URL` | `http://backend:8080` | Backend URL (Docker service name; accessible inside Docker network) |
| `REVALIDATE_SECRET` | `dev-revalidate-secret` | Must match backend value; used to authenticate `/api/revalidate` calls |

### Backend (`backend` service)

| Variable | Dev value | Description |
|----------|-----------|-------------|
| `DATABASE_URL` | `postgres://postgres:pass@db:5432/suratpedia?sslmode=disable` | PostgreSQL connection string |
| `REDIS_URL` | `redis:6379` | Redis address (no protocol prefix) |
| `R2_ACCOUNT_ID` | `your-cf-account-id` | Cloudflare Account ID |
| `R2_ACCESS_KEY` | `your-r2-access-key` | R2 Access Key ID |
| `R2_SECRET_KEY` | `your-r2-secret-key` | R2 Secret Access Key |
| `R2_BUCKET` | `suratpedia-storage` | R2 bucket name |
| `R2_PUBLIC_URL` | `https://r2.suratpedia.com` | Public base URL for generated file links |
| `PARSER_URL` | `http://parser:8001` | Internal URL of Python parser service |
| `JWT_SECRET` | `dev-jwt-secret-at-least-32-chars-long` | JWT signing key |
| `ADMIN_EMAIL` | `admin@suratpedia.com` | Admin login email |
| `ADMIN_PASSWORD` | `dev-password` | Admin login password |
| `REVALIDATE_SECRET` | `dev-revalidate-secret` | Must match frontend value |

### Parser (`parser` service)

No environment variables required. Configuration is only in `requirements.txt` and `Dockerfile`.

## Local Services

| Service | Container name | Data volume | Reset command |
|---------|---------------|-------------|---------------|
| PostgreSQL 16 | `suratpedia-db-1` | `pgdata` | `docker-compose down -v && docker-compose up -d` |
| Redis 7 | `suratpedia-redis-1` | `redisdata` | `docker-compose exec redis redis-cli FLUSHDB` |

Useful dev commands:
```bash
# Access PostgreSQL
docker-compose exec db psql -U postgres -d suratpedia

# Inspect Redis cache keys
docker-compose exec redis redis-cli KEYS "dl:*"

# Flush Redis cache (all keys)
docker-compose exec redis redis-cli FLUSHDB

# Rebuild a single service after code change
docker-compose up -d --build backend

# Reset entire database (destroys all data)
docker-compose down -v
docker-compose up -d
```

## Parser Development Note

The parser Docker image is ~500MB because it bundles LibreOffice (required for DOCX→PDF conversion). Initial `docker-compose up` will take a few minutes on first pull/build.
