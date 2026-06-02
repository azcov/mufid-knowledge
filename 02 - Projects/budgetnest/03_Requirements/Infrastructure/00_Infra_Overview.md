---
tags: [infrastructure, architecture]
---

# Infrastructure Overview — BudgetNest

## System Diagram
```
Browser / Mobile
    │
    ▼
Next.js (Vercel) ────────────────────────────────
    │
    ▼
Go API (Railway / Oracle VM, port 8080)
    ├── PostgreSQL (Supabase, ap-southeast-1)
    ├── Supabase Auth (JWT + webhook)
    ├── Supabase Storage (receipts)
    ├── Xendit (billing webhooks)
    ├── Anthropic (receipt scan)
    └── Resend (email)

Caddy (reverse proxy, prod only)
    └── TLS termination → Go API
```

## Services
| Service | Role | Host |
|---------|------|------|
| Next.js | Web UI | Vercel |
| Go API | Business logic, REST | Railway (dev) / Oracle VM + Caddy (prod) |
| PostgreSQL | Primary DB | Supabase (Singapore) |
| Supabase Auth | Auth + JWT | Supabase |
| Supabase Storage | Receipt images | Supabase |
| Xendit | Subscription billing | Xendit API |
| Anthropic | Receipt scan AI | Anthropic API |
| Resend | Transactional email | Resend API |

## Data Flow — Payment
1. User selects plan → Go creates Xendit invoice
2. User pays → Xendit sends `invoice.paid` webhook
3. Go validates `XENDIT_WEBHOOK_TOKEN` (constant-time)
4. Updates `subscriptions` → unlocks paid features server-side

## External Dependencies
| Service | Free Tier | Paid Trigger |
|---------|-----------|-------------|
| Supabase | 500MB DB, 1GB storage | > 500MB DB → $25/mo |
| Railway | Dev free | Production → ~$5-10/mo |
| Vercel | 100GB bandwidth | Overage |
| Resend | 3,000 emails/mo | > 3K → $20/mo |
