---
tags: [requirements, specs]
---

# Functional Specs — BudgetNest

## Core Features

### Expense Logging
- Amount + category + account + optional note
- Target: < 5 seconds per entry
- Auto-timestamp, editable
- Shared instantly to household members (paid)

### Budget Categories
- 10 system defaults in Bahasa Indonesia (Makanan, Transport, Tagihan, dll)
- Custom categories by user
- Free tier: capped at 5 total; Paid: unlimited
- Each category has monthly budget target

### Monthly Budget Plan
- Set target per category at month start
- Real-time actual vs plan display
- Alert at 80% (Phase 2)

### AI Receipt Scan
- Photo → auto-extract amount + category
- Free: 5 scans/month; Paid: unlimited
- Powered by Anthropic vision API

### Shared Household Ledger (Paid)
- Up to 4 members per household
- All members see all expenses in real time
- Invite by email or link
- Household is the multi-tenancy unit

### Reports
- Monthly: total spent, per-category breakdown vs budget
- Phase 2 (paid): month-over-month trend, CSV export

## User Flows

### Log an Expense
1. Tap "+" → enter amount → pick category → pick account → optional note → save
2. Budget updates instantly; shared to household members

### Onboarding
1. Sign up (email/Google) → create household → set categories → set monthly budgets
2. Invite partner (optional)

### Upgrade
1. Hit free limit → "Upgrade" prompt → pick plan → Xendit payment → unlock instantly

### AI Receipt Scan
1. Tap scan → camera opens → snap receipt → AI returns amount + category suggestion → confirm/edit → logged

## Edge Cases
- Cross-household data isolation: all queries filter by `household_id`
- Freemium limits enforced at usecase layer (never just frontend)
- Xendit webhook: idempotent — duplicate `invoice.paid` must not double-unlock
- If paid member's subscription lapses, household drops to free tier limits
