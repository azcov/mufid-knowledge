---
tags: [requirements, specs]
---

# Functional Specs

## Core Features

### 1. Template Browse & Discovery
- Homepage shows hero section (H1 from DB SEO metadata), search bar, category grid, and 8 featured/popular templates
- Category pages list templates with pagination (20 per page), filter pills for all categories, and an SEO intro paragraph
- Template detail shows: format badge, free/premium badge, popular badge, download count, view count, rating, HTML preview, DOCX + PDF download buttons, tags
- `view_count` increments automatically on every `GET /api/templates/:slug` call

### 2. Download Flow (lazy generation + caching)
1. User clicks DOCX or PDF button on a template detail page
2. If template has variables: a modal form opens, user fills in fields (text, date, or image upload per variable definition)
3. Frontend sends `POST /api/download` with `{ template_id, format, variables }`
4. Backend generates a deterministic cache key: `MD5(templateId + format + sortedJSON(variables))`
5. Cache lookup order: Redis → DB `download_cache` table → generate fresh
6. On cache miss: fetch template JSON from DB → call Python parser `POST /generate` → upload bytes to Cloudflare R2 → save to DB + Redis
7. Response contains a public R2 URL; frontend triggers `window.location = download_url` to start browser download

### 3. Variable Substitution
- Templates can contain `{{variable_name}}` placeholders in TipTap JSON content
- Variable definitions are stored as JSONB on the `templates` table: key, label, type (text/date/image), required flag
- The download modal renders dynamic form fields based on this array
- Variables are sorted alphabetically before hashing to ensure cache key consistency

### 4. AI Generator Wizard
- Route: `/generator`
- 4-step wizard:
  1. Select category (grid of category icons)
  2. Select template within category
  3. Fill variable form (same dynamic form as download modal)
  4. Download result (DOCX or PDF)
- Client-side only (no server-side rendering needed); state managed with `useState`

### 5. Search
- Route: `/cari?q=...` (implied by SearchBar component behaviour)
- Backend: `GET /api/templates/search?q=` — searches by title, `content_text`, and tags
- Pagination supported (page + limit)

### 6. Admin Panel
All admin pages are Client Components with a 401-redirect auth guard.

| Page | Route | Function |
|------|-------|---------|
| Login | `/admin/login` | Email + password → JWT cookie |
| Dashboard | `/admin` | Stats overview, nav links |
| Template list | `/admin/templates` | Table with edit/delete actions |
| Upload template | `/admin/templates/new` | Drag-drop file + metadata form |
| Edit template | `/admin/templates/[id]` | Edit title, slug, format, active, featured, premium |
| SEO list | `/admin/seo` | Table of all SEO metadata entries |
| Edit SEO | `/admin/seo/[slug]` | Full SEO form with live SERP + OG preview |

### 7. SEO Management
- Every category and template page can have a custom `seo_metadata` DB record with: meta title (60 char), meta description (160 char), OG tags, Twitter Card, canonical URL, robots directive, JSON-LD schema markup
- `generateMetadata()` in Next.js fetches from `/api/seo/{slug}` server-side on each request (cached via ISR)
- Admin can write BreadcrumbList and FAQPage schema in the `schema_markup` JSONB field
- SERP Preview and OG Preview components give real-time visual feedback in the SEO editor

### 8. Tiered File Lifecycle (Cron)
Runs daily at 03:00:
- **Short tier** (< 10 downloads): expires after 7 days of no access, deleted from R2 + DB + Redis
- **Medium tier** (10–99 downloads): expires after 30 days of no access, deleted from R2 + DB
- **Permanent tier** (≥ 100 downloads): moved from `/temp/` to `/templates/` in R2, never deleted
- Auto-upgrade to permanent also fires as an async goroutine during any download that crosses the 100-download threshold

## User Flows

### Flow A: Anonymous user downloads a template without variables
```
Homepage → click category → Category page → click template →
Template detail → click "Download DOCX" →
POST /api/download { template_id, format: "docx", variables: {} } →
Response: { download_url: "https://r2.suratpedia.com/..." } →
Browser downloads file
```

### Flow B: Anonymous user downloads a template with variables
```
Template detail → click "Download DOCX" →
Modal opens with dynamic form (e.g. "Nama Lengkap", "Perusahaan Tujuan", "Tanggal") →
User fills fields → Submit →
POST /api/download { template_id, format: "docx", variables: { nama_pengirim: "Budi", ... } } →
Response: { download_url } →
Browser downloads personalised file
```

### Flow C: Generator wizard
```
/generator →
Step 1: pick "Surat Lamaran Kerja" →
Step 2: pick specific template →
Step 3: fill variable form →
Step 4: download DOCX or PDF
```

### Flow D: Admin uploads a new template
```
/admin/templates/new →
Drag-drop DOCX file, fill title + slug + category →
Submit (multipart POST /api/admin/templates) →
Backend forwards file to POST /parse on Python parser →
Parser returns { content_json, variables, metadata } →
Backend inserts into templates table →
Redirect to template list
```

### Flow E: Admin edits SEO metadata
```
/admin/seo → find page slug → Edit →
/admin/seo/[slug] →
Update meta_title, meta_description, og_image, schema_markup →
SERP Preview updates in real time →
Save (PUT /api/admin/seo/:slug) →
Backend triggers POST /api/revalidate to Next.js to flush ISR cache
```

## Edge Cases

- **Cache key collision**: Variables sorted alphabetically before JSON serialisation to prevent `{a:1,b:2}` and `{b:2,a:1}` producing different keys for the same output
- **Template with no variables, downloaded 100+ times**: On the 100th download, a goroutine runs CopyObject from `/temp/{cache_key}.ext` to `/templates/{id}.ext` on R2 without blocking the response
- **Expired file requested again**: Cache miss at Redis + DB will cause a fresh generation — the file is regenerated transparently
- **Admin deletes template**: Soft delete (sets `is_active = false`); public API filters by `is_active = true`, so the template disappears from public pages without losing DB history
- **Parser unavailable**: Backend returns `500 DOWNLOAD_ERROR`; no retry logic documented
