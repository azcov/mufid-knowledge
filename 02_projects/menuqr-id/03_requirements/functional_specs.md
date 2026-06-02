---
tags: [requirements, specs]
---
# Functional Specs — MenuQR

## Core Features

### Menu Builder (Owner)
- Create categories and items (name, photo, price, description)
- Mark items as sold-out (live, no QR reprint needed)
- Multiple menus per outlet (e.g., food menu + drink menu)
- Upload outlet logo (paid)

### QR Generation
- Auto-generate QR code linking to menu page
- Download as print-ready PDF (sticker size)
- QR URL: `menu.qr/{outlet-slug}` or custom subdomain (Pro)

### Customer Menu Page
- Mobile web (no app install)
- Clean layout: photos, prices, categories
- "Habis" badge on sold-out items
- "Pesan via WhatsApp" CTA → opens WA with pre-filled order message

### WhatsApp Order Message (Auto-filled)
```
Halo, saya ingin memesan:
- Nasi Ayam Bakar x2
- Es Teh x1
Total estimasi: Rp 55.000
Terima kasih!
```

### Analytics (Pro)
- QR scans per day/week
- Most viewed items
- Peak ordering times

## User Flow — Owner Setup
1. Register → create outlet → add menu items with photos
2. Download QR PDF → print and laminate
3. Place QR on tables → done

## User Flow — Customer
1. Scan QR → menu page opens instantly (no login)
2. Browse → tap "Pesan via WhatsApp"
3. WhatsApp opens with pre-filled order → send
4. Owner receives structured order on WhatsApp

## Key Design Principle
"Owners must succeed without a tech person" — setup in 15 minutes on one phone.
