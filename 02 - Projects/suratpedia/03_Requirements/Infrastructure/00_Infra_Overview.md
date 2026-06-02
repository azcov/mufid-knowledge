---
tags: [infrastructure, architecture]
---

# Infrastructure Overview

## System Map

```
                    ┌──────────────────────────────────────┐
                    │            INTERNET                   │
                    └──────────────────┬───────────────────┘
                                       │
                    ┌──────────────────▼───────────────────┐
                    │         FRONTEND  :3000               │
                    │         Next.js 15 (App Router)       │
                    │                                       │
                    │  Public (ISR)        Admin (CSR)      │
                    │  /                   /admin           │
                    │  /[category]         /admin/templates │
                    │  /[category]/[slug]  /admin/seo       │
                    │  /generator                           │
                    └──────────────────┬───────────────────┘
                                       │ HTTP (NEXT_PUBLIC_API_URL)
                    ┌──────────────────▼───────────────────┐
                    │         BACKEND  :8080                │
                    │         Go Fiber v2                   │
                    │                                       │
                    │  /api/categories   /api/templates     │
                    │  /api/seo          /api/download      │
                    │  /api/admin/*                         │
                    └──────┬─────────┬──────────┬──────────┘
                           │         │          │
           ┌───────────────▼──┐ ┌────▼────┐ ┌──▼─────────────┐
           │  PostgreSQL 16   │ │ Redis 7 │ │  Parser :8001  │
           │  :5432           │ │ :6379   │ │  Python FastAPI │
           │                  │ │         │ │                │
           │  categories      │ │ dl:*    │ │  POST /parse   │
           │  templates       │ │ (URLs)  │ │  POST /generate│
           │  seo_metadata    │ └─────────┘ └────────────────┘
           │  download_cache  │
           │  template_images │
           └──────────────────┘
                                       │ S3 API (R2_* env vars)
                    ┌──────────────────▼───────────────────┐
                    │         CLOUDFLARE R2                 │
                    │  Bucket: suratpedia-storage           │
                    │  Public URL: r2.suratpedia.com        │
                    │                                       │
                    │  /templates/{id}.docx   (permanent)   │
                    │  /templates/{id}.pdf                  │
                    │  /temp/{cache_key}.docx (TTL 7-30d)   │
                    │  /temp/{cache_key}.pdf                │
                    │  /images/logos/...                    │
                    │  /images/signatures/...               │
                    │  /images/stamps/...                   │
                    └──────────────────────────────────────┘
```

## Services & Responsibilities

| Service | Role | Host | Port |
|---------|------|------|------|
| Frontend (Next.js 15) | SSR/ISR public pages, CSR admin panel, calls backend API | Docker container | 3000 |
| Backend (Go Fiber v2) | REST API, business logic, download service, cron jobs | Docker container | 8080 |
| Parser (Python FastAPI) | DOCX/PDF parsing (DOCX → TipTap JSON), file generation (TipTap JSON → DOCX/PDF bytes) | Docker container | 8001 |
| PostgreSQL 16 | Primary relational database (templates, categories, seo, cache metadata) | Docker container | 5432 |
| Redis 7 | Download URL cache (key: `dl:{cache_key}`, TTL by tier) | Docker container | 6379 |
| Cloudflare R2 | Object storage for generated files and letterhead images | External SaaS | — |

## Data Flow

### User downloads a file
1. Browser → `POST /api/download` to Backend
2. Backend checks Redis (`dl:{cache_key}`) — if hit, return URL immediately (< 50ms)
3. Backend checks `download_cache` table in PostgreSQL — if found + not expired, warm Redis and return URL
4. On full miss: Backend fetches template JSON from PostgreSQL → calls `POST /generate` on Parser → receives binary bytes → uploads to R2 → stores URL in PostgreSQL + Redis → returns URL
5. Frontend receives URL → `window.location = download_url` → browser downloads directly from R2 (no backend streaming)

### Admin uploads a template
1. Admin multipart POST to `/api/admin/templates`
2. Backend sends file bytes to `POST /parse` on Parser
3. Parser returns TipTap JSON + variable definitions + metadata
4. Backend inserts record into PostgreSQL `templates` table
5. Backend calls `/api/revalidate` on Next.js frontend to flush ISR cache

### Nightly cleanup (03:00)
1. Cron in Backend queries `download_cache` for expired short-tier and inactive medium-tier files
2. Deletes from R2, then from PostgreSQL `download_cache`, then from Redis
3. Queries templates with ≥ 100 downloads not yet on permanent tier — moves files from `/temp/` to `/templates/` in R2

## External Dependencies

| Dependency | Purpose | Notes |
|------------|---------|-------|
| Cloudflare R2 | File storage | S3-compatible; requires Account ID, Access Key, Secret Key; needs a custom domain set up (`r2.suratpedia.com`) |
| Cloudflare CDN (optional) | Proxy / CDN for frontend | Mentioned in production checklist but not required for local/staging |
| LibreOffice (inside Parser container) | DOCX → PDF conversion | Bundled in the Python Docker image (~500MB); no external service call |
