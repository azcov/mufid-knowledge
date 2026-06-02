---
tags: [requirements, architecture]
---

# Technical Architecture

## Stack

| Layer | Technology | Version | Why |
|-------|-----------|---------|-----|
| Frontend | Next.js (App Router) | 15 | ISR native, `generateMetadata()` for server-side SEO |
| Frontend styling | Tailwind CSS + shadcn/ui | latest | Rapid UI, consistent design system |
| Backend | Go + Fiber v2 + sqlx | Go 1.22 | High performance, low latency for simple API operations |
| Database | PostgreSQL | 16 | JSONB support for TipTap JSON content, full relational integrity |
| Cache | Redis | 7 | Native TTL, O(1) key lookup, no DB query per cached download |
| Storage | Cloudflare R2 | — | S3-compatible API, zero egress cost — critical for download-heavy workload |
| Parser | Python + FastAPI | Python 3.11 | python-docx and pdfplumber are the most mature DOCX/PDF libraries |
| Auth | JWT | — | Admin-only, no end-user accounts needed |
| Deploy | Docker Compose | — | Simple single-server deploy |

## Data Model

### `categories`
Flat category list (parent_id exists for future sub-categories but unused). Key fields: `name`, `slug`, `icon` (emoji), `sort_order`, `is_active`.

### `templates`
Core table. Content stored as TipTap JSON in three JSONB columns:
- `content_json` — letter body
- `header_json` — kop surat (letterhead)
- `footer_json` — footer

Plain text (`content_text`) stored separately for full-text search. HTML (`content_html`) stored for fast preview rendering without re-parsing JSON.

Variable placeholders use `{{key}}` syntax. The `variables` JSONB column stores an array of `{ key, label, type, required }` objects. `has_variables` boolean shortcut avoids deserialising the array just to check.

File lifecycle fields: `docx_url`, `pdf_url` (null until first download), `generated_at`, `last_accessed`, `download_tier` (`short` / `medium` / `permanent`).

### `seo_metadata`
Standalone table, slug-based lookup (not FK to templates/categories). Covers: meta title/description, Open Graph, Twitter Card, canonical URL, robots directive, JSON-LD schema markup (`schema_markup` JSONB), page H1 and intro paragraph.

### `download_cache`
Tracks generated files. Primary key lookup by `cache_key` (MD5 hash). Stores R2 URLs (docx_url, pdf_url), tier, expiry timestamp (NULL = permanent), and download count.

Cache key formula: `MD5(str(template_id) + format + json_sorted_by_key(variables))`

### `template_images`
Stores metadata for images embedded in letter content (logos, signatures, stamps). Images are stored in R2 at `images/logos/`, `images/signatures/`, `images/stamps/`. SHA256 checksum field enables deduplication.

### Relationships
```
categories ──< templates >── template_images
              templates ──< download_cache
seo_metadata (standalone, slug-based)
```

### Recommended Indexes
```sql
CREATE INDEX idx_templates_slug       ON templates(slug);
CREATE INDEX idx_templates_category   ON templates(category_id) WHERE is_active = true;
CREATE INDEX idx_templates_featured   ON templates(is_featured, download_count DESC) WHERE is_active = true;
CREATE INDEX idx_cache_key            ON download_cache(cache_key);
CREATE INDEX idx_cache_expires        ON download_cache(tier, expires_at) WHERE expires_at IS NOT NULL;
CREATE INDEX idx_seo_slug             ON seo_metadata(slug);
```

## API Design

Base URL: `http://localhost:8080`

All error responses: `{ "error": "message", "code": "ERROR_CODE" }`  
Pagination default: `?page=1&limit=20`

### Public endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/categories` | All active categories with template counts |
| GET | `/api/categories/:slug` | Single category detail |
| GET | `/api/templates` | Paginated template list (featured + popular first) |
| GET | `/api/templates/search?q=` | Search by title, content_text, tags |
| GET | `/api/templates/:slug` | Template detail (also increments view_count) |
| GET | `/api/seo/:slug` | SEO metadata for any page slug |
| POST | `/api/download` | Lazy generate or return cached DOCX/PDF URL |

### Admin endpoints (JWT cookie required)

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/admin/login` | Authenticate, set httpOnly JWT cookie |
| POST | `/api/admin/logout` | Clear cookie |
| GET | `/api/admin/templates` | All templates including inactive |
| POST | `/api/admin/templates` | Upload template file (multipart), triggers parser |
| PUT | `/api/admin/templates/:id` | Update template metadata |
| DELETE | `/api/admin/templates/:id` | Soft delete (sets is_active=false) |
| GET | `/api/admin/seo` | All SEO metadata entries |
| POST | `/api/admin/seo` | Create SEO entry |
| PUT | `/api/admin/seo/:slug` | Update SEO entry |
| POST | `/api/admin/invalidate-cache/:id` | Flush Redis cache for a template |

### Download API contract

Request:
```json
POST /api/download
{
  "template_id": 42,
  "format": "docx",
  "variables": { "nama_pengirim": "Budi Santoso", "kota": "Jakarta" }
}
```

Response (cache miss):
```json
{ "status": "generated", "cache_hit": false, "download_url": "https://r2.suratpedia.com/temp/abc123.docx", "expires_at": "...", "expires_in": 604800, "generated_ms": 2340 }
```

Response (cache hit):
```json
{ "status": "cached", "cache_hit": true, "download_url": "https://r2.suratpedia.com/temp/abc123.docx", "expires_at": "...", "expires_in": 597200, "generated_ms": 45 }
```

### Parser service API (internal)

Base URL: `http://parser:8001`

| Method | Path | Input | Output |
|--------|------|-------|--------|
| POST | `/parse` | multipart file (DOCX or PDF) | `{ content_json, header_json, footer_json, variables, images, metadata }` |
| POST | `/generate` | `{ content_json, header_json, footer_json, variables, format }` | Binary file bytes |

## Infrastructure

See [[03_Requirements/Infrastructure/00_Infra_Overview]] for the full system map.

Key architectural constraints:
- Backend never streams file bytes to the client; it returns R2 URLs only
- Parser is a sidecar that can be scaled independently
- All public frontend pages use ISR (category: 300s revalidate, SEO: 3600s revalidate)
- Admin pages are pure Client Components (CSR) with 401-redirect auth guard
- Content language: Bahasa Indonesia throughout
