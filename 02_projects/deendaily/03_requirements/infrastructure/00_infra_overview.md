---
tags: [infrastructure, architecture]
---

# Infrastructure Overview — DeenDaily

## System Diagram
```
React Native App (Expo)
    │
    ▼
Go API (Render.com, Singapore, port 8080)
    ├── PostgreSQL (Neon)
    ├── Neon Auth (JWKS JWT validation)
    ├── Stripe (IAP receipt validation)
    └── App Store / Play Store IAP
```

## Services
| Service | Role | Host |
|---------|------|------|
| React Native | iOS + Android app | App Store / Play Store |
| Go API | Business logic, habit tracking | Render.com (Singapore) |
| PostgreSQL | Primary DB | Neon |
| Neon Auth | JWT JWKS validation | Neon |
| Stripe | Subscription validation | Stripe API |
| App Store IAP | iOS payments (30%→15%) | Apple |
| Play Store IAP | Android payments | Google |

## Key Difference from Other Projects
- Uses **Render.com** (not Railway) — has `render.yaml` config
- Uses **Neon Auth** (JWKS) instead of Supabase Auth (HS256 secret)
- **No Xendit** — payments via App Store / Google Play IAP only (required for mobile apps)
- Has `docker-compose.yml` for local dev (api + web only, no DB — external Neon)
