---
tags: [research, infrastructure, solo-dev, hosting]
source: claude-convertation
date: 2026-05-26
---

# Infrastructure for Solo Dev Startup

Source: "Infrastruktur untuk startup solo developer" (2026-05-26)

## Prinsip
**Mulai simpel, bayar sesuai pakai, hindari over-engineering.**

## Stack Architecture (Next.js + Golang)
```
[User]
  │
  ▼
Cloudflare (DNS + CDN + DDoS)
  │
  ├── /          → Vercel (Next.js — SSR/SSG)
  └── /api/*     → Railway / Fly.io (Golang API)
                        │
                  ┌─────┴──────┐
               Supabase     Upstash
              (Postgres)    (Redis)
```

## Near-$0 Stack (Free Tier)

### Frontend — Next.js
| Platform | Free Tier | Batasan |
|----------|-----------|---------|
| **Vercel** | Unlimited deploy, 100GB BW, 6000 build min/mo | Default pilihan terbaik |
| **Cloudflare Pages** | Unlimited BW, 500 build/mo, unlimited sites | Alternatif terbaik |
| **Netlify** | 100GB BW, 300 build min/mo | Lebih terbatas |

### Backend — Golang API
| Platform | Free Tier | Batasan |
|----------|-----------|---------|
| **Railway** | $5 credit/mo free | Terbaik untuk Go, Singapore region |
| **Render** | 750 jam/mo free | Spin down setelah 15 menit idle |
| **Fly.io** | 3 shared-CPU VM gratis | Generous, flexible regions |
| **Hetzner VPS** | ~€4/bln (CX22, 2 vCPU, 4GB) | Paling murah untuk full control |

### Database
| Platform | Free Tier | Harga Paid |
|----------|-----------|-----------|
| **Supabase** | 500MB Postgres, 1GB Storage | Pro $25/mo |
| **Neon** | 0.5GB, auto-pause | Pro $19/mo |
| **PlanetScale** | 5GB (closed free tier 2024) | $39/mo |
| **Turso** | 9GB, 500 DBs | $29/mo |

### Auth
| Platform | Free Tier |
|----------|-----------|
| **Supabase Auth** | 50K users free |
| **Better Auth** | Self-hosted, gratis |
| **Clerk** | 10K MAU free |

### Storage
| Platform | Free Tier |
|----------|-----------|
| **Cloudflare R2** | 10GB/mo free, $0.015/GB after |
| **Supabase Storage** | 1GB free |
| **Backblaze B2** | 10GB free |

### Email
| Platform | Free Tier |
|----------|-----------|
| **Resend** | 3,000 emails/mo free |
| **Brevo (ex-Sendinblue)** | 300 emails/day free |
| **Mailersend** | 3,000 emails/mo free |

### Analytics
| Platform | Free Tier |
|----------|-----------|
| **Umami** | Self-host free (Vercel + Supabase) |
| **Plausible** | $9/mo (no free tier but affordable) |
| **Google Analytics** | Free |

### Monitoring
| Platform | Free Tier |
|----------|-----------|
| **UptimeRobot** | 50 monitors, 5 min interval |
| **Better Stack** | 3 URLs free |
| **Sentry** | 5K events/mo free |

## Best $0/Month Combination
**Vercel + Fly.io + Supabase + Better Auth + Umami + R2 + Resend + UptimeRobot**

Upgrade path: Railway ($5–10/mo) untuk backend setelah ada revenue.
