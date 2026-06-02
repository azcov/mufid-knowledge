---
tags: [requirements, architecture]
---
# Technical Architecture — WaCRM

## Stack (Planned)
- Frontend: Next.js (App Router), mobile-first
- Backend: Go + Gin
- Database: PostgreSQL (Supabase, Singapore)
- Auth: Supabase Auth
- Payments: Xendit
- Notifications: email (Resend) + in-app
- Host: Railway (backend) + Vercel (frontend)

## Key Entities
- `users` — Supabase mirror
- `workspaces` — business account (multi-tenant)
- `workspace_members` — user ↔ workspace
- `contacts` — name, phone, tags, status, notes
- `interactions` — log entries per contact (manual notes)
- `reminders` — scheduled follow-up per contact
- `pipelines` — custom stages (Business tier)
- `subscriptions` — Xendit subscription state

## Phase 2 Addition
- `whatsapp_messages` — synced via WhatsApp Business API
- `wa_threads` — conversation threads per contact
