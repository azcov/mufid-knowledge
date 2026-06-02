---
tags: [management, scope]
---

# Scope & Goals

## Goal

Build Suratpedia (suratpedia.com) — the most complete Indonesian letter template directory. Users browse categories, preview letter content, fill in personalisation variables (nama, tanggal, perusahaan, etc.), and download a ready-to-use DOCX or PDF file. An admin panel lets a single operator upload templates and manage SEO metadata per page.

## In Scope

### Public-facing features
- Homepage with category grid and popular templates
- Category listing pages (`/[category]`) with pagination and filter pills
- Template detail page (`/[category]/[slug]`) with content preview and download buttons
- AI Generator wizard (`/generator`) — 4-step flow: pick category → pick template → fill variables → download
- Search by title, content text, and tags

### Admin features
- Admin login with JWT (httpOnly cookie)
- Template list: view all (including inactive), soft-delete, edit metadata
- Upload template: accept DOCX/PDF, trigger parser, auto-create DB record
- Edit template: title, slug, format, is_active, is_featured, is_premium flags
- SEO Manager: per-page meta title, description, Open Graph, Twitter Card, canonical URL, robots, JSON-LD schema markup
- SERP Preview and OG Preview in the SEO editor
- Manual cache invalidation per template

### Infrastructure / platform
- Lazy DOCX/PDF generation via Python parser service
- Tiered file caching (short / medium / permanent) in Cloudflare R2
- Redis caching layer for download URLs
- Nightly cron (03:00) to clean expired files and upgrade popular templates
- ISR revalidation endpoint for on-demand cache invalidation after admin edits
- Full SEO stack: per-page metadata, canonical URLs, BreadcrumbList schema, FAQPage schema

### Template categories (seed data)
1. Surat Lamaran Kerja
2. Surat Pengunduran Diri
3. Surat Kuasa
4. Surat Resmi
5. Surat Pernyataan
6. Surat Pribadi
7. Surat Dinas
8. Surat Jual Beli Tanah

## Out of Scope

- User accounts / registration (there is no end-user login; admin only)
- Payment processing or subscription management (premium flag exists in schema but monetisation flow is not built)
- Template editing by end users (read + download only; LetterEditor component exists but is read-only for public)
- Multi-language UI (all UI text is in Bahasa Indonesia)
- Mobile app
- Social sharing features (beyond OG meta tags)
- Sub-category hierarchy (parent_id column exists in schema but is unused)
- Multi-admin roles (single ADMIN_EMAIL / ADMIN_PASSWORD in env)

## Success Metrics

| Metric | Target | Notes |
|--------|--------|-------|
| Template catalogue | 1,200+ templates at launch | Mentioned in homepage SEO seed copy |
| Download latency (cache hit) | < 50ms response | Redis hit target |
| Download latency (cache miss) | < 3s end-to-end | Python generation + R2 upload |
| SEO indexation | All category pages indexed with unique meta title/desc | Managed via DB, no hardcoded SEO |
| Storage efficiency | Popular files (100+ downloads) on permanent tier, rest pruned by cron | Tiered caching system |
| Admin usability | Single operator can add template in < 5 minutes | Upload → parse → publish flow |
