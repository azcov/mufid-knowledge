---
tags: [requirements, specs]
---
# Functional Specs — Invoice App

## Core Invoice Flow
1. Create invoice: line items (description, qty, unit price), tax %, discount, notes, due date
2. Auto-number (INV-0001, INV-0002...)
3. Save as draft → review → send
4. Client receives email with payment link

## Client Payment Portal
- No account required for client
- Shows invoice details, sender name/logo, all payment methods
- QRIS, Virtual Account, GoPay, OVO, Dana, ShopeePay, BNPL, credit card
- Client picks method → pays → confirmation page shown
- Freelancer gets notified (email + in-app)

## Invoice Status Flow
```
draft → sent → viewed (client opens link) → paid / overdue
```
Auto-transition: overdue if unpaid after due date.
Auto-reminder: email to client at due_date + 3 days.

## Payment Methods
All via Xendit or Midtrans (single integration):
- QRIS (universal — any Indonesian e-wallet/bank that supports QRIS)
- Virtual Account: BCA, Mandiri, BNI, BRI
- GoPay / OVO / Dana / ShopeePay
- BNPL: Kredivo, Akulaku
- Credit/debit card
User selects which methods to enable on their account.

## Transaction Fee
Platform fee % deducted from payment before disbursement.
User sees expected net payout when enabling each payment method.
Transparent pricing — no hidden fees.

## Key Design: Client Experience
Clean, branded payment page with client's choice of payment method.
No new account. One click to pay. Should feel as easy as Tokopedia checkout.
