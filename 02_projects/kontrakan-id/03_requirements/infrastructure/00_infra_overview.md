---
tags: [infrastructure]
---
# Infrastructure Overview — Kontrakan / Kostify

## System Diagram
```
Expo Mobile + Next.js Web
    │ Bearer JWT
    ▼
Go API (chi, port 8080)  + Goroutine Scheduler (1hr tick)
    ├── PostgreSQL (Supabase, Singapore)
    ├── Supabase Auth (OTP + Google)
    ├── Supabase Storage (tenant docs, Pro)
    ├── Fonnte (WhatsApp gateway, Pro tier only)
    └── Xendit (subscription billing + webhooks)
```

## Services
| Service | Role | Notes |
|---------|------|-------|
| Go API | Business logic | chi router, goroutine scheduler |
| Supabase | DB + Auth + Storage | Singapore region |
| Fonnte | WA automated reminders | Pro tier only; cheapest Indonesian WA gateway |
| Xendit | Subscription billing | QRIS, bank transfer, e-wallets |

## Fonnte vs WhatsApp Business API
Fonnte is used **instead of** the official WhatsApp Business API because:
- WA Business API requires Meta approval (slow, expensive)
- Fonnte works for individual landlords (lower cost, simpler)
- Trade-off: less reliable at scale; plan to migrate at v3
