---
tags: [management, roadmap]
---

# Roadmap — Suratpedia

## Done ✅ (from sessions ~2026-05-29)
- Full implementation from `.planning/product/product.md`
- Multi-agent approach: separate FE and BE parallel agents
- Documentation written in `.planning/`
- Seeder created for letter template categories
- `.claude/settings.local.json` configured (`dontAsk` mode for dev)

## Stack Implemented
| Layer | Tech |
|-------|------|
| Frontend | Next.js 15, TypeScript, Tailwind, shadcn/ui |
| Backend | Go 1.22, Fiber v2, sqlx |
| Parser service | Python 3.11, FastAPI |
| DB | PostgreSQL 16 + Redis 7 |
| Storage | Cloudflare R2 |
| Auth | JWT (admin, httpOnly cookie) |
| Deploy | Docker Compose |

## Services Running
| Service | Port |
|---------|------|
| Frontend | 3000 |
| Backend | 8080 |
| Parser | 8001 |
| PostgreSQL | 5432 |
| Redis | 6379 |

## Key Architecture
- **Lazy generation**: templates generated on first download, not on upload
- **Tiered storage**: temp (7-day) → permanent R2 (after 100 downloads)
- Parser: TipTap JSON format → PDF/DOCX generation
- Admin panel at `/admin`

## Planned 📅
- SEO landing pages per category + per letter type
- QRIS payment for premium downloads
- Letter customization (fill-in fields before download)
- Email delivery option (send letter via email)
