---
tags: [environment, staging]
---

# Staging Environment — BudgetNest

## Hosting
| Service | Platform |
|---------|----------|
| Backend | Railway staging service |
| Frontend | Vercel Preview (auto per PR) |
| Database | Supabase staging project (separate from prod) |

## Key Rules
- Never share production DB with staging
- Use Xendit test/sandbox mode for payments
- Every PR gets a Vercel preview URL automatically
- Supabase staging project should be in same region (Singapore) for realistic latency
