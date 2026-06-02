---
tags: [management, budget]
---

# Budget & Resources

## Infrastructure Costs (Target)

The hard financial guardrail from the CEO master plan:

| Metric | Target |
|--------|--------|
| Infrastructure cost | ≤ 15% of MRR |
| Cost per MAU | ≤ Rp 400/month all-in |
| Cost per safety scan | ≤ Rp 12 |
| Pro gross margin | ≥ 70% |
| Free-tier cost per user | < Rp 500/month |

## Hosting & Compute

| Service | Tier | Estimated Cost |
|---------|------|---------------|
| Render (Go API backend) | Starter web service | ~$7/month prod; $0 dev |
| Vercel (Next.js frontend) | Hobby/Pro | Free tier (100GB bandwidth) → Pro when scaling |
| Supabase (PostgreSQL + Auth + Storage) | Free tier | $0 (500MB DB, pauses after inactivity on free) |
| Upstash (Redis for cache + rate limiting) | Serverless | Pay-per-request; generous free tier |
| Cloudflare (CDN, planned Phase 021) | Free tier initially | $0 until significant traffic |

Alternative considered: Koyeb for backend (no cold starts, better for Go), Neon for Postgres (serverless, branching). Documented in `.planning/architecture/mvp-infra.md`.

## External Services

| Service | Purpose | Tier |
|---------|---------|------|
| Supabase Auth | Email/password + Google OAuth | Free tier |
| Supabase Storage | Product images | Free tier |
| ImageKit | Image CDN / transformation | Free tier |
| Xendit | Payment processing (QRIS, GoPay, OVO, DANA, BCA VA) | Pay per transaction |
| PostHog | Product analytics | Free tier (1M events/month) |
| Sentry or GlitchTip | Error tracking | Free tier |
| Better Stack | Uptime monitoring + log search (3-day retention free) | Free tier |
| Resend | Transactional email | Free tier (100 emails/day) |

## AI / LLM Costs

AI is NOT in the safety scorer hot path. Costs are constrained to:

- Nightly batch: fuzzy ingredient name matching via GPT-4o-mini (~$0.15 per 10k strings)
- Phase 031+ only: Pro-tier ingredient explanations
- Phase 038: Fine-tuned local SLM to reduce AI costs 80% vs. external APIs

## Pricing Model

| Tier | Price | Features |
|------|-------|---------|
| Free | Rp 0 | Basic product search, BPOM + halal badge, limited safety checks (5/session) |
| Pro | Rp 29.000/month | Unlimited safety checks, barcode scanner, saved products, WhatsApp share cards, PDF export |
| Pro Annual | Rp 249.000/year | Pro features at 28% discount vs. monthly |

Payment methods (Indonesia-first): QRIS, GoPay, OVO, DANA, BCA Virtual Account, Mandiri Virtual Account, credit cards. Stripe only from Phase 044+ for international expansion.

## Time Allocation

Not documented. Solo founder project.
