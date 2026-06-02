---
tags: [provisioning, infrastructure]
---

# Provisioning

Note: Despite the file name, Suratpedia does not use AWS. All infrastructure is self-hosted Docker Compose + Cloudflare R2.

## Cloud / Hosting

**Platform**: Self-hosted VPS (no specific provider documented)  
**Orchestration**: Docker Compose (`docker-compose.yml` at repo root)  
**No managed cloud services** — all services run as Docker containers on a single host.

## Compute

All five services are defined as Docker Compose services:

| Service | Image / Build | Port Mapping | Notes |
|---------|--------------|--------------|-------|
| `frontend` | Built from `./frontend` | `3000:3000` | Multi-stage: deps → builder → runner (standalone Next.js) |
| `backend` | Built from `./backend` | `8080:8080` | Multi-stage: Go build → alpine:3.19; static binary, image < 20MB |
| `parser` | Built from `./parser` | `8001:8001` | python:3.11-slim + LibreOffice; image ~500MB |
| `db` | `postgres:16` | `5432:5432` | Data persisted in `pgdata` named volume |
| `redis` | `redis:7-alpine` | `6379:6379` | Data persisted in `redisdata` named volume |

**Startup dependency order** (healthcheck-gated):
```
db (pg_isready healthy, 10 retries × 5s) → backend
redis → backend
backend → frontend
```

**Frontend build (multi-stage Dockerfile)**:
- Stage 1 (deps): `npm install --legacy-peer-deps`
- Stage 2 (builder): `npm run build` with `NEXT_TELEMETRY_DISABLED=1`
- Stage 3 (runner): `node server.js` using `output: "standalone"` from `next.config.ts`

**Backend build (multi-stage Dockerfile)**:
- Stage 1: `CGO_ENABLED=0 GOOS=linux go build -o server ./cmd/main.go`
- Stage 2: `alpine:3.19` + `ca-certificates` + `tzdata`

**Parser Dockerfile**:
```dockerfile
FROM python:3.11-slim
RUN apt-get install -y libreoffice
RUN pip install -r requirements.txt
```

## Storage

### PostgreSQL Volumes
- Named volume: `pgdata` mounted at `/var/lib/postgresql/data`
- Database name: `suratpedia`
- Initialised from `./db/init.sql` on first start (Docker entrypoint convention)
- Init script runs: CREATE TABLEs + seed categories + seed SEO metadata + 2 sample templates

### Redis Volumes
- Named volume: `redisdata` mounted at `/data`

### Cloudflare R2
- **Bucket**: `suratpedia-storage`
- **Public domain**: `r2.suratpedia.com` (custom domain mapped to bucket)
- **Access**: S3-compatible API via AWS SDK v2 in the Go backend
- **Endpoint format**: `https://{R2_ACCOUNT_ID}.r2.cloudflarestorage.com`

**R2 bucket structure**:
```
suratpedia-storage/
├── templates/          ← permanent files (id-based path, never deleted)
│   ├── 42.docx
│   └── 42.pdf
├── temp/               ← temporary files (cache_key-based path, TTL 7-30d)
│   ├── a3f8c2d1.docx
│   └── a3f8c2d1.pdf
└── images/             ← letterhead assets
    ├── logos/
    ├── signatures/
    └── stamps/
```

**Path logic**:
- `has_variables = true` → store in `temp/{cache_key}.{ext}`
- `has_variables = false` → store in `templates/{id}.{ext}`
- `download_count >= 100` → move from `temp/` to `templates/` (CopyObject + DeleteObject)

**R2 setup steps** (from deployment docs):
1. Create bucket `suratpedia-storage` in Cloudflare dashboard
2. Enable public access or set custom domain
3. Map `r2.suratpedia.com` to the bucket
4. Create API token with "Object Read & Write" permission
5. Copy Account ID, Access Key, Secret Key into backend env vars

**R2 CORS config** (optional, only if browser needs direct R2 access):
```json
[{ "AllowedOrigins": ["https://suratpedia.com"], "AllowedMethods": ["GET"] }]
```

## Networking

- All services communicate over Docker Compose's default bridge network using service names (`db`, `redis`, `parser`, `backend`)
- Only ports 3000, 8080, and 8001 are mapped to the host
- PostgreSQL (5432) and Redis (6379) are not exposed outside the Docker network in production
- Frontend calls backend using `NEXT_PUBLIC_API_URL` (set to backend service name inside Docker, or the public backend URL in production)

## IaC

**Docker Compose** is the sole IaC tool. No Terraform, Pulumi, or Kubernetes configurations documented.

Key environment variables injected at runtime (see [[03_Requirements/Infrastructure/Environments/Dev_Config]] for full list):
- `DATABASE_URL`, `REDIS_URL` — internal Docker network addresses
- `R2_ACCOUNT_ID`, `R2_ACCESS_KEY`, `R2_SECRET_KEY`, `R2_BUCKET`, `R2_PUBLIC_URL`
- `JWT_SECRET`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`
- `REVALIDATE_SECRET` (shared between frontend and backend for ISR revalidation)
- `PARSER_URL` — `http://parser:8001`
