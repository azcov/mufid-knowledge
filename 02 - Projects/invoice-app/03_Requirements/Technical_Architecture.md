---
tags: [requirements, architecture]
---
# Technical Architecture — Invoice App

## Stack (Planned)
| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14+ (App Router) |
| Backend | Go + Gin |
| Database | PostgreSQL (Supabase, Singapore) |
| Auth | Supabase Auth (email + Google OAuth) |
| Payments | Xendit or Midtrans (QRIS, VA, GoPay, OVO, Dana, BNPL, cards) |
| PDF | Go PDF library (gofpdf or chromedp/headless) |
| Email | Resend (invoice delivery + overdue reminders) |
| Host | Vercel (frontend) + Railway (backend) |

## Payment Gateway Choice: Xendit vs Midtrans
- Xendit: simpler API, used across other labs projects
- Midtrans: more established in Indonesia (GoTo-owned), wider e-wallet coverage
Decision TBD — benchmark both for fee structure.

## Signed Link Architecture
Payment links are token-gated:
`/pay/{invoice_id}?token={signed_token}`
- Token signed with invoice secret + user secret
- 30-day expiry
- View-only after expiry (can't pay, can still see)

## Key Entities
- `users` — freelancers/agencies
- `clients` — contact address book
- `invoices` — draft/sent/viewed/paid/overdue
- `invoice_items` — line items
- `payments` — Xendit/Midtrans transaction records
- `payment_disbursements` — net payout to user after fee
