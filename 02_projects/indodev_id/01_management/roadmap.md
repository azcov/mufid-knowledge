---
tags: [management, roadmap]
---

# Roadmap — IndoDev-ID

## Done ✅ (from sessions ~2026-05-31)
- Full planning docs written in `docs/`
- Frontend: Next.js + Supabase client initialized
- Backend: Go + Gin structure

## Known Issue (fixed in session)
- Frontend: `Invalid supabaseUrl` error — fixed by ensuring env vars properly set

## In Progress 🔧
- Core frontend: job search, listing pages, apply flow
- Backend: job posts API, company API, developer profile API
- Manual seeding strategy for 200+ initial listings

## Planned 📅

### Phase 1 MVP: Q3–Q4 2026
**Developer (free):** Profile, search/browse (with salary filter), apply, status tracking
**Company (paid boosts):** Post (salary required), applicant management, featured/email blast/candidate search
**Cold start:** Manual seed 200+ listings with salary ranges

**Gate:** 10K devs, 200+ listings, 50 companies with ≥ 1 paid boost

### Phase 2: Q1 2027
- Developer public profile, job alerts, company subscription tier
- SEO programmatic pages ("Remote Go Jobs Indonesia")

### Phase 3: Q2–Q3 2027
- Indonesian developer salary survey
- Developer portfolio showcase
- Community forum (requires 30K+ devs)

## Infrastructure Notes
- DB: Supabase free tier (note: limited to 2 active projects on free plan)
- Alternative free PostgreSQL: Neon (0.5GB, auto-pause), Turso (9GB, 500 DBs)
- Frontend: Vercel, Backend: Railway
