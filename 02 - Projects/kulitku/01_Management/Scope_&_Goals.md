---
tags: [management, scope]
---

# Scope & Goals

## Goal

Ship a mobile-responsive web app that lets Indonesian consumers — especially pregnant and breastfeeding mothers — check skincare product safety in under 60 seconds by combining BPOM registration status, halal certification, and per-ingredient safety scoring in one place, in Bahasa Indonesia.

## In Scope (MVP)

- Product catalog: 100 manually curated skincare products
- Ingredient safety scoring per user skin profile (life stage: pregnant, breastfeeding, parent_of_child, allergy, general)
- BPOM registration badge (with evidence link to cekbpom.pom.go.id)
- Halal MUI badge (with evidence link to halalmui.org)
- Skin profile management: multiple profiles per user (e.g., "Saya - Hamil", "Anak Budi")
- Doctor's forbidden ingredient list per profile (score = 0 on match, hard block)
- Admin panel: CRUD for products, ingredients, ingredient aliases, badges
- Supabase Auth: email/password + Google OAuth
- Mobile-responsive web (375px, 768px, 1440px breakpoints)
- Safety score: 0-10 with verdict (Aman / Perhatian / Hindari) + flagged ingredient list in Bahasa Indonesia
- WhatsApp-shareable safety card (OG image via @vercel/og)
- Product search by name/brand (tsvector + pg_trgm for typo tolerance)
- Barcode scanner (@zxing/browser EAN-13)
- Pro subscription via Xendit: Rp 29.000/month or Rp 249.000/year
- Feature gating: server-side via Redis-backed rate limiter (paywall at 4th barcode scan, 6th safety check, save-product, PDF export)
- SEO foundation: sitemap, structured data (JSON-LD), generateMetadata

## Out of Scope (MVP)

- Native mobile app (React Native is post-MVP)
- Automated BPOM scraper or BPOM API integration
- Community / review features
- Doctor consultation feature
- Affiliate links / e-commerce integration
- Coverage outside Indonesia (BPOM/halal is Indonesia-specific)
- Precomputed safety scores cache (planned Phase 015, but after launch)
- Brand portal / B2B certified badge program (Phase 034+)
- Multi-tenancy

## Success Metrics

| Metric | Day-30 Target | Day-60 Target |
|--------|--------------|--------------|
| Paying subscribers | 50 (Rp 1.45M MRR) | 500 (Rp 14.5M MRR) |
| K-factor (viral coefficient) | — | ≥ 0.5 |
| Cache hit ratio (post-Phase 015) | — | ≥ 90% |
| Cost per MAU | — | < Rp 400/month |
| Pro gross margin | — | > 70% |
| Customer payback period | — | < 6 months |
| Search p99 latency | < 200ms | < 200ms |

## Non-Goals (permanent)

- No ads — brand advertising conflicts with trust in safety verdicts
- No false SAFE ratings on doctor-forbidden ingredients — precision over recall
- No country expansion before Phase 020 unit economics gate passes
