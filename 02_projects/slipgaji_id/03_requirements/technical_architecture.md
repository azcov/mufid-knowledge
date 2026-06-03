---
tags: [requirements, architecture]
---
# Technical Architecture — SlipGaji

## Stack (Planned)
- Frontend: Next.js (App Router), desktop-optimized
- Backend: Go + Gin
- Database: PostgreSQL (Supabase)
- PDF generation: Go PDF library (gofpdf or chromedp)
- Payments: Xendit

## Key Entities
- `companies` — employer accounts
- `employees` — per company, with full salary structure
- `payroll_periods` — month + year per company
- `payslips` — generated slip per employee × period (with PDF URL)
- `subscriptions` — Xendit subscription state

## PPh 21 TER Calculation
Critical: Indonesia's 2024 DJP regulation shift to TER method.
Must be accurate and auditable — wrong calculation = compliance risk.
Calculation at usecase layer with explicit formula per bracket.
