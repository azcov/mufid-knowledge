---
tags: [research, skincare, kulitku, product-research]
source: claude-convertation
date: 2026-05-04
---

# Skincare Ingredient Platform Research

Source: "Indonesian skincare product listing platform" (2026-05-04)

## Reference Platforms
| Platform | Market | Focus |
|----------|--------|-------|
| **SkinSafe** | Global/US | Ingredient safety for sensitive skin |
| **EWG Skin Deep** | Global/US | Safety ratings, health concerns |
| **Skincarisma** | Global | Ingredient checker + product comparison |
| **CosDNA** | Global | Acne-trigger detection, comedogenic analysis |
| **INCI Decoder** | Global | In-depth ingredient analysis |

## Indonesia-Specific Opportunity
- **None of these are Bahasa Indonesia-first**
- **None focus on BPOM registration** (Indonesian regulatory body)
- **None address halal certification** (critical for Indonesian Muslim consumers)
- **Key deadline:** Indonesia BPJPH halal certification deadline October 2026 → creates urgency

## Core Differentiators for Kulitku
1. **Bahasa Indonesia** — all content in Indonesian
2. **BPOM verification** — check product's BPOM registration number
3. **Halal certification check** — BPJPH/MUI halal status
4. **Safety for pregnant/breastfeeding** — specific concern for mothers
5. **Barcode scanning** — scan product in-store via ZXing browser barcode scanner

## Product Architecture Notes
- MVP: 100 manually curated products
- Barcode scanning via `@zxing/browser` in frontend
- Safety score computation at usecase layer (on-demand)
- INCI parsing via Python FastAPI (similar to suratpedia parser service)

## Related: Dual Brand Strategy (from memories)
- **Kulitku** — Indonesian brand, Indonesian market
- **Dermiq** — Global variant, same backend, different frontend
- Monorepo + feature flags for dual-brand management

→ See [[../../02 - Projects/kulitku/00_Project_Hub]]
