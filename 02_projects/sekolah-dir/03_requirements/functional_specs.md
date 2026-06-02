---
tags: [requirements, specs]
---
# Functional Specs — Sekolah-Dir

## Parent Features (Free)
- Search: city, district, level (Playgroup/TK/SD/SMP/SMA), type (negeri/swasta/international), curriculum (Kurikulum Merdeka/Cambridge/IB/pesantren), fee range
- School listing: name, accreditation, curriculum, fee range, photos, contact, map embed, reviews
- Comparison shortlist: save up to 5, compare side-by-side
- Reviews: star rating (overall + per dimension) + written review, OTP-verified phone
- Map view: "schools near me" via browser geolocation

## School Features (Paid)
- **Claim flow:** submit NPSN + SKTM or business doc → admin approves < 48h
- **Basic:** edit name, description, photos (up to 10), fee range, programs, contact; verified badge; priority sort
- **Premium:** featured card (top of results), "Tanya Sekolah Ini" lead capture, analytics dashboard (views/WA clicks/leads), promo banner

## Admission Inquiry Lead (Premium)
1. Parent clicks "Tanya Sekolah Ini" on Premium listing
2. Fills: name, phone, child's current grade
3. School receives lead in dashboard → contacts parent directly

## Data Seeding Strategy
1. Import NPSN/Dapodik (Kemendikbud open data) — 300K+ schools: name, NPSN, address, level, type, accreditation
2. Manually enrich top 1,000 schools in 10 target cities: photos, fee range, curriculum, WhatsApp contact

## SEO Architecture
- School pages: `/sekolah/{city}/{slug}`
- Schema.org markup on all school pages
- ISR for enriched listings; static for unenriched
