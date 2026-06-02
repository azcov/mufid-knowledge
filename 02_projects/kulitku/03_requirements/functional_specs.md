---
tags: [requirements, specs]
---

# Functional Specs

## Core Features

### 1. Product Safety Check (core value prop)

User searches for a product by name or brand, selects their skin profile, and receives:
- Safety score 0-10
- Verdict: Aman (8-10, green) / Perhatian (4-7, yellow) / Hindari (0-3, red) / Data belum lengkap (null, grey)
- Flagged ingredients in Bahasa Indonesia with reason
- Hard red warning if doctor-forbidden ingredient found (score = 0, no partial scoring)
- BPOM registration badge with evidence URL
- Halal MUI badge with evidence URL and expiry date

### 2. Skin Profile Management

- Users can create multiple profiles per account (e.g., "Saya - Hamil", "Anak Budi 2 tahun")
- One profile can be set as default
- Profile fields: name, life_stage (pregnant / breastfeeding / parent_of_child / allergy / general), skin_type (oily / dry / combination / sensitive / normal), skin_concerns (JSONB array), forbidden_ingredient_ids (doctor's list)
- Safety scoring always done against a specific profile

### 3. Product Search

- Search by product name or brand
- tsvector full-text search + pg_trgm trigram index for typo tolerance
- p99 search latency target: < 200ms

### 4. Barcode Scanner

- EAN-13 barcode scan via device camera (@zxing/browser)
- Look up product by barcode field
- Pro-tier feature: paywall triggers at 4th scan

### 5. WhatsApp Share Card

- Every safety check produces a shareable PNG card via @vercel/og (Satori)
- Card shows: product image, score gauge, Bahasa verdict, Kulitku watermark + QR code
- One-tap share: whatsapp://send on mobile, web.whatsapp.com on desktop
- Every share link is a UTM-tagged short URL (kulk.it/{code}) for K-factor tracking

### 6. Subscription / Paywall

- Paywall triggers at moments of maximum intent: 4th barcode scan, 6th safety check, save-product click, PDF export click
- 7-day free trial (requires payment method upfront to eliminate passive trialists)
- Feature gating is server-side via Redis-backed middleware (never client-side)
- Pro: Rp 29.000/month or Rp 249.000/year

### 7. Admin Panel (internal)

Admin users (`app_role = "admin"` in JWT) have access to:
- Ingredient CRUD + alias management (add/edit/delete aliases per ingredient)
- Product CRUD + ingredient list editor (ordered, drag-and-drop by position)
- Badge template management
- Product badge assignment with evidence URLs and expiry dates

## User Flows

### New User: First Safety Check

1. Open kulitku.id landing page
2. Search for product by name or brand
3. View product detail → prompted to sign up / sign in to see full safety score
4. Sign up via Supabase Auth (email or Google)
5. 3-step onboarding: choose life stage → add forbidden ingredients (optional) → choose skin type
6. Profile persists; redirect back to product detail
7. See full safety score card with verdict + flagged ingredients in Bahasa Indonesia
8. See one-tap WhatsApp share button on score card

### Returning User: Barcode Scan

1. Open app (already signed in)
2. Tap barcode scanner icon
3. Camera opens, scan product barcode (EAN-13)
4. Product detail opens immediately
5. Safety score shown for default profile
6. 4th scan → paywall prompt

### Admin: Add a New Product

1. Sign in as admin
2. Open /admin/products → Add New Product
3. Fill name, brand, BPOM number, barcode, image URL
4. Navigate to ingredient list editor → add raw label text for each ingredient (ordered by position on label)
5. System resolves aliases to canonical ingredient_id (NULL if not yet in DB)
6. Navigate to badge assignments → assign BPOM badge with evidence URL → assign Halal badge with cert number + expiry
7. Product immediately searchable by users

## Safety Score Algorithm

```
score = 10
verdict = "safe"
flagged = []

for each ingredient in product_ingredients (ordered by position):
  if ingredient_id in skin_profile.forbidden_ingredient_ids:
    → score = 0, STOP (hard block, add to forbidden_found)

  safety = ingredient.safety_by_profile[life_stage] ?? "safe"

  if safety == "avoid":
    score -= 3, verdict = "avoid", add to flagged
  if safety == "caution" and verdict != "avoid":
    score -= 1, verdict = "caution", add to flagged

return max(score, 0)
```

Score is `null` when all product_ingredients have ingredient_id = NULL (catalog not yet populated).

## Edge Cases

| Case | Behavior |
|------|---------|
| Ingredient not in DB | ingredient_id = NULL, raw_label_text preserved; shown as "belum terdata" to user |
| All ingredients unmatched | Score = null / "Data belum lengkap", no verdict |
| Forbidden ingredient found | Score = 0, stop immediately, show hard red warning regardless of other ingredients |
| User has no skin profile | Default to life_stage = 'general'; prompt to create profile for personalized score |
| Badge expired | Status = 'expired', shown with "Kadaluarsa" label (not hidden) |
| Alias matches multiple ingredients | Should not happen; enforced at app layer before insert (unique lower(alias)) |
| life_stage key missing from safety_by_profile | Defaults to "safe" at runtime; admin must fill all keys |
| 7 products checked, 4th barcode scan, or save-product | Paywall shown; user can still see limited info |

## Feature Gating (Free vs. Pro)

| Feature | Free | Pro |
|---------|------|-----|
| Product search | Yes | Yes |
| BPOM + halal badge | Yes | Yes |
| Safety score | 5 checks/session limit | Unlimited |
| Barcode scanner | 3 scans | Unlimited |
| WhatsApp share card | Yes | Yes |
| Saved products | No | Yes |
| PDF safety report export | No | Yes |
| Multiple skin profiles | 1 | Unlimited |
