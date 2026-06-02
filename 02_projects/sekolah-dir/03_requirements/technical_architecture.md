---
tags: [requirements, architecture]
---
# Technical Architecture — Sekolah-Dir

## Stack (Planned — matches other labs projects)
| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14+ (SSR/ISR), Bahasa Indonesia default |
| Backend | Go + Gin |
| Database | PostgreSQL (Supabase, Singapore) |
| Auth | Supabase Auth (email + Google) |
| Payments | Xendit (school subscriptions) |
| Ads | Google AdSense (parent-facing pages) |
| Host | Vercel (frontend) + Railway (backend) |

## Key Entities
- `schools` — from NPSN seed + enrichment (name, NPSN, city, level, type, curriculum, fee range, accreditation)
- `school_photos` — enriched photos
- `school_accounts` — claimed school owner
- `subscriptions` — Basic or Premium, Xendit-backed
- `reviews` — parent reviews (OTP-verified phone)
- `inquiry_leads` — Premium: parent → school lead capture
- `analytics_events` — views, WA clicks, lead clicks per school

## Cold Start
Primary competitive advantage: seeding 300K+ schools from Dapodik day 1. No competitor has structured, filterable data at this scale.
