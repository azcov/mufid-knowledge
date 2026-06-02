---
tags: [requirements, architecture]
---

# Technical Architecture — DeenDaily

## Stack
| Layer | Technology |
|-------|-----------|
| Mobile | React Native (Expo) — iOS + Android |
| Backend | Go + Gin |
| Database | PostgreSQL via Neon (Neon Auth for JWT) |
| Auth | Neon Auth (JWKS-based JWT) + Supabase Auth |
| Payments | App Store IAP + Google Play IAP (server-side receipt validation via Stripe) |
| Frontend host | Vercel (web, post-MVP) |
| Backend host | Render.com (Singapore, starter plan) |

## Render.com Config (render.yaml)
```yaml
services:
  - type: web
    name: deendaily-api
    runtime: docker
    dockerfilePath: ./backend/Dockerfile
    region: singapore
    plan: starter
    healthCheckPath: /health
    envVars:
      - key: PORT / ENV / LOG_LEVEL / DB_URL
      - key: NEON_AUTH_JWKS_URL
      - key: STRIPE_SECRET_KEY / STRIPE_WEBHOOK_SECRET / STRIPE_PREMIUM_PRICE_ID
      - key: APP_URL / ALLOWED_ORIGINS
```

## Architecture Pattern
`handler → usecase → repository` (same as all labs projects)

## Auth Flow
1. User signs in via Neon Auth (email/Google/Apple)
2. Mobile sends `Authorization: Bearer <jwt>` on every call
3. Go middleware validates JWT via Neon Auth JWKS endpoint
4. First login: creates `users` row in DB

## Key Entities
- `users` — Neon Auth mirror
- `habits` — per user (salah + sunnah preset + custom)
- `habit_logs` — daily check-in entries (habit × date × status)
- `streaks` — computed or cached streak per user
- `subscriptions` — IAP subscription state (validated server-side)
