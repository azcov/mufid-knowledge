---
tags: [requirements, specs]
---

# Functional Specs — Bengkelterdekat

## Core Features

### Public Directory (No Login)
- Search by city + keyword (workshop name, service type)
- Filter: city, specialization tag, price range (Budget/Mid/Premium), car brand
- Workshop listing card: name, city, top 3 tags, price range, verified badge, thumbnail
- Workshop detail page: full info, photos, hours, phone, WhatsApp deep link, Google Maps embed
- AdSense display ads (non-blocking, async)

### SEO Pages (Programmatic)
- `/bengkel/[tag]/[city]` — e.g., `/bengkel/kaki-kaki/surabaya`
- `/bengkel/[city]` — city overview
- 140+ pages at launch; canonical URLs, breadcrumb schema, JSON-LD
- Sitemap.xml auto-generated (all listing + SEO pages)

### Admin Panel
- Workshop CRUD (name, address, city, phone, tags, price range, cover photo)
- Bulk CSV import for initial seeding
- Claim queue: view, approve, reject with note (< 48h turnaround)
- Specialization tag management
- Subscription management

### Owner Dashboard (Paid)
- Edit: description, photos (up to 10), specialization tags, service packages, promo banner, hours, contact
- Analytics: daily view count, phone taps, WhatsApp taps (simple event tracking, stored in DB)
- Subscription management: view status, cancel, upgrade to annual

### Payment (Xendit)
- Rp 149K/month or Rp 1.49M/year
- Methods: QRIS, bank transfer, GoPay, OVO, DANA
- Webhook: `invoice.paid` → activate subscription (idempotent)

## User Flows

### Car Owner: Find a Workshop
1. Homepage → enter city + problem keyword (e.g., "suspensi") or browse by tag
2. Search results with filters → click workshop card
3. Detail page: see specialization, photos, price range, contact info
4. Tap WhatsApp deep link → contact workshop directly (no account needed)

### Workshop Owner: Claim Listing
1. Find own listing → click "Klaim Bengkel Ini"
2. Fill: owner name, phone, role, photo evidence (at workshop location)
3. Admin reviews → approved within 48h
4. Pay via Xendit → owner dashboard activated
5. Edit listing, upload photos, post promos

## Edge Cases
- AdSense approval: requires live site with sufficient content before applying
- Duplicate listing prevention: admin must check before inserting
- Xendit webhook idempotency: `invoice.paid` duplicate should not re-extend subscription
