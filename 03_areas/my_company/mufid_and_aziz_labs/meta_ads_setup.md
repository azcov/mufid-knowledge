---
tags: [company, marketing, meta-ads, facebook, instagram]
---

# Meta Ads Setup

## How Meta's Structure Works

```
Meta Business Manager (1 — Mufid & Aziz Labs)
│
├── Ad Accounts (1–3 max for now)
│   ├── Ad Account #1 — shared across products
│   └── Ad Account #2 — separate if needed (e.g. high spend product)
│
├── Facebook Pages (1 per product)
│   ├── Page: Suratpedia
│   ├── Page: Bengkelterdekat
│   ├── Page: SlipGaji
│   └── Page: Mufid & Aziz Labs (studio)
│
├── Instagram Accounts (connected to Pages)
│   └── 1 IG per Page
│
└── Pixels (1 per product website)
    ├── Pixel: suratpedia.com
    ├── Pixel: bengkelterdekat.com
    └── Pixel: slipgaji.id
```

---

## Short Answer

| Thing | How many | Notes |
|-------|----------|-------|
| **Business Manager** | 1 | One umbrella for everything |
| **Ad Accounts** | 1 to start, add per product later | See below |
| **Facebook Pages** | 1 per product | Free, create when product launches |
| **Instagram Accounts** | 1 per Page | Linked to the Page |
| **Pixels** | 1 per product website | Critical for tracking conversions |

---

## Ad Account Strategy

### Option A — 1 Shared Ad Account (Recommended to start)
Use one ad account for all products. Different campaigns per product inside.

**Pros:**
- Simpler to manage
- Ad spend history builds on one account (helps algorithm learn faster)
- Less admin overhead

**Cons:**
- If account gets flagged/banned, all products affected
- Harder to separate billing per product

**When to use:** When you're just starting ads, spending < Rp 10–20M/month total.

---

### Option B — Separate Ad Account per Product (Later)
Each product that runs consistent ads gets its own ad account.

**Pros:**
- Isolated risk — one ban doesn't kill all products
- Clean billing and reporting per product
- Easier to hand off to a hire later

**Cons:**
- More accounts to manage
- Each account starts with zero history (algorithm needs to relearn)
- Meta limits new Business Managers to 2–5 ad accounts initially

**When to use:** When a product is spending Rp 20M+/month consistently, or when you hire a dedicated growth person for it.

---

## Pixel Setup (Important)

Each product needs its own **Meta Pixel** installed on the website.
This tracks: page views, sign ups, purchases, and lets you build lookalike audiences.

```
suratpedia.com      → Pixel ID: xxxx
bengkelterdekat.com → Pixel ID: yyyy
slipgaji.id         → Pixel ID: zzzz
```

Install via:
- Next.js: `next/script` with the pixel base code
- Or use **Meta's CAPI (Conversion API)** from the backend for more accurate tracking
  (server-side, not blocked by ad blockers)

---

## Recommended Setup Order

### Step 1 — Now (before any product launches)
- [ ] Create **Meta Business Manager** at business.facebook.com
- [ ] Add payment method (credit card / GoPay)
- [ ] Create **1 Ad Account** — "Mufid & Aziz Labs"
- [ ] Create **Mufid & Aziz Labs Facebook Page** (studio brand)

### Step 2 — When first product launches (Suratpedia / Bengkelterdekat)
- [ ] Create Facebook Page for the product
- [ ] Connect Instagram account to the Page
- [ ] Create and install **Pixel** on product website
- [ ] Run first campaign from shared ad account

### Step 3 — When product hits Rp 20M+ MRR
- [ ] Create dedicated Ad Account for that product
- [ ] Move campaigns there
- [ ] Consider hiring growth person to manage

---

## Meta Ads Budget Reality for Indonesian SME SaaS

| Phase | Monthly budget | Goal |
|-------|---------------|------|
| Testing | Rp 500K–2M/product | Find what works — don't scale yet |
| Validation | Rp 3–10M/product | Scale what works |
| Growth | Rp 10–50M/product | Only after organic works too |

**Rule:** Don't run paid ads until you have organic traction first.
Ads amplify what's already working — they don't fix a broken funnel.

---

## Account Safety Tips

- Use your **real personal Facebook account** as the Business Manager admin
- Add a backup admin (trusted friend or alternate account)
- **Never** use a fake/bot account — Meta bans fast and permanently
- Don't run misleading ads or make health/medical claims (Kulitku risk)
- Keep billing info updated — expired card = suspended account

---

## Links
- [[social_media_strategy]] — organic social first, ads second
- [[roadmap]] — when to invest in paid growth
