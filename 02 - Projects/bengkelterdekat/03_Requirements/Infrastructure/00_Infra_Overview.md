---
tags: [infrastructure, architecture]
---

# Infrastructure Overview — Bengkelterdekat

## System Diagram
```
Car Owner Browser (Next.js SSR/SSG on Vercel)
    │
    ▼
Go API (Railway/ECS, port 8080)
    ├── PostgreSQL (Supabase, Singapore)
    ├── Supabase Auth (JWT)
    ├── Supabase Storage (workshop photos)
    ├── Xendit (subscription billing webhooks)
    └── Google AdSense (client-side, async)
```

## Services
| Service | Role | Host |
|---------|------|------|
| Next.js | Public directory (SSR/ISR/SSG) + admin panel | Vercel |
| Go API | Backend REST (chi + pgx) | Railway (dev) / AWS ECS (prod) |
| PostgreSQL | Workshop listings, tags, subscriptions | Supabase (Singapore) |
| Supabase Auth | Admin + owner auth | Supabase |
| Supabase Storage | Workshop photos | Supabase |
| Xendit | Workshop owner subscriptions | Xendit API |
| Google AdSense | Display ads on public pages | Client-side JS |

## Data Flow — SEO Page
1. Bot or visitor hits `/bengkel/kaki-kaki/surabaya`
2. Next.js ISR serves pre-rendered HTML from Vercel edge
3. Search engines index structured data + content
4. No API call needed for cached/static pages

## Data Flow — Xendit Payment
1. Workshop owner submits claim → admin approves
2. Owner clicks "Bayar" → Go creates Xendit invoice
3. Owner pays → Xendit sends `invoice.paid` webhook
4. Go validates, activates subscription in DB
