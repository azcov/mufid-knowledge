---
tags: [management, budget]
---

# Budget & Resources — BudgetNest

## Infrastructure Costs (Monthly Estimate)

| Service | Tier | Est. Cost |
|---------|------|-----------|
| Supabase | Free → Pro ($25/mo) | $0–25/mo |
| Railway | Dev free / Prod ~$5 | $0–10/mo |
| Vercel | Free (100GB BW) | $0 |
| Xendit | % per transaction | Revenue share |
| Anthropic | Pay-per-token | ~$5–20/mo (receipt scans) |
| Resend | Free 3K/mo → $20 | $0–20/mo |
| Sentry | Free → ~$26/mo | $0–26/mo |
| Total (early) | | ~$30–80/mo |

## Tools & Services
| Tool | Purpose |
|------|---------|
| Supabase | Auth + PostgreSQL + Storage |
| Railway | Go API hosting |
| Vercel | Next.js hosting |
| Xendit | Subscription billing (QRIS, GoPay, OVO, DANA, VA) |
| Anthropic | AI receipt scan |
| Resend | Transactional email |
| Sentry | Error tracking |
| GitHub Actions | CI/CD |
| GHCR | Docker image registry (prod) |

## Time Allocation
- Backend: Go API (core business logic, freemium gating, billing)
- Frontend: Next.js (expense logging, dashboard, budget plan)
- Mobile: Post-PMF
