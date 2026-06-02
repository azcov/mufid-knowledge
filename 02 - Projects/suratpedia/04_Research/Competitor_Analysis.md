---
tags: [research, competitors]
---

# Competitor Analysis

No formal competitor analysis is included in the planning documentation. The product docs describe Suratpedia's positioning and target user but do not name specific competitors.

The following analysis is inferred from the product context and the Indonesian letter template search market.

## Market Context

Suratpedia targets Indonesians searching for letter templates (surat) via Google. The homepage SEO seed copy claims "1,200+ template surat gratis" and targets keywords like "contoh surat lamaran kerja 2025", "template word gratis". This is a high-volume, SEO-driven niche.

## Likely Competitor Categories

| Type | Description | Suratpedia differentiation |
|------|-------------|---------------------------|
| Blog-style sites (e.g. surat.id, contohsurat.co.id) | Publish letter examples as HTML articles; no download functionality | Suratpedia offers actual DOCX/PDF download with variable substitution, not just reading |
| Generic document sites (e.g. scribd.com) | Global platforms, not Indonesia-specific, no variable fill | Suratpedia is Indonesia-specific, free, with personalisation wizard |
| Word template marketplaces | Paid downloads, not search-optimised for Indonesian keywords | Suratpedia is free and built for Indonesian SEO traffic |
| Google Docs template gallery | English-first, general-purpose | Suratpedia covers Indonesian letter formats (kop surat, TTD, stempel) |

## Suratpedia's Differentiation

Based on the planning docs, the key differentiators are:

1. **Variable substitution**: Users fill in their own name, company, date etc. and get a personalised DOCX/PDF — not a generic template they have to edit manually.
2. **Format fidelity**: TipTap JSON format preserves kop surat (letterhead) structure, table-based headers, embedded logos and signatures — common in Indonesian formal letters.
3. **SEO-first architecture**: Every category page has custom meta title, description, Open Graph, and JSON-LD schema managed from the admin panel. Designed to rank for high-intent keywords.
4. **Free egress via R2**: Using Cloudflare R2 means file downloads cost nothing in bandwidth fees, enabling truly free downloads at scale.
5. **Tiered caching**: Popular templates (100+ downloads) are served instantly from permanent R2 storage; obscure templates are lazily generated only on demand, keeping storage costs low.

## Gaps vs. Competitors (from planning docs)

- No user ratings system implemented yet (schema exists, UI not built)
- No user accounts (competitors with accounts can offer "my saved templates")
- Single admin operator model — content volume depends on one person uploading templates
