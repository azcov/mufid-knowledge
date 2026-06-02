---
tags: [environment, prod]
---

# Production — Bengkelterdekat

## Hosting
| Service | Platform |
|---------|----------|
| Go API | Railway / AWS ECS |
| Next.js | Vercel (auto-deploy from `main`) |
| DB | Supabase production (Singapore) |
| Photos | Supabase Storage |

## Deploy Checklist
1. `make test` passes
2. `make lint` clean
3. `make build` succeeds
4. `make migrate-up` on prod DB
5. All env vars set in Railway + Vercel dashboards
6. `GET /health` returns 200
7. Google Search Console — no new indexing errors
8. AdSense serving on at least 1 placement

## SEO Monitoring
- Google Search Console: check after every deploy (sitemap, coverage, core web vitals)
- Lighthouse: SEO score > 80 on listing pages
