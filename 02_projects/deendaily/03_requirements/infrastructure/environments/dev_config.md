---
tags: [environment, dev]
---

# Dev Environment — DeenDaily

## Setup
```bash
# Backend
cd backend
cp .env.example .env  # fill DB_URL (Neon dev branch) + Neon auth vars
docker compose up -d  # starts api on port 8080

# Mobile
cd mobile
npx expo start  # Expo Go on device/simulator
```

## Env Vars
| Var | Required |
|-----|----------|
| `PORT` | ✅ 8080 |
| `ENV` | ✅ development |
| `DB_URL` | ✅ Neon dev branch connection string |
| `NEON_AUTH_JWKS_URL` | ✅ Neon Auth JWKS endpoint |
| `STRIPE_SECRET_KEY` | optional (blank = skip IAP validation) |
| `STRIPE_WEBHOOK_SECRET` | optional |
| `STRIPE_PREMIUM_PRICE_ID` | optional |
| `APP_URL` | optional |
| `ALLOWED_ORIGINS` | set to Expo dev URL |
