---
tags: [infrastructure]
---
# Infrastructure Overview — IndoDev-ID

## Hosting
- Frontend: Vercel (Next.js, SEO-optimized SSR)
- Backend: Go API on Railway (Gin + pgx, Singapore)
- DB: PostgreSQL on Supabase (Singapore)
- Auth: Supabase Auth
- Payments: Xendit (boost purchases)

## SEO Architecture
- SSR/ISR for all job listing and search pages
- Programmatic SEO: "Remote React Developer Jobs Indonesia", "Backend Go Jobs Jakarta", etc.
- Sitemap auto-generated, updated on new job posts
