---
tags: [security, auth, iam]
---

# Security & Auth — DeenDaily

## Auth Strategy
- Provider: Neon Auth (JWKS-based)
- Token: JWT validated against Neon JWKS endpoint (`NEON_AUTH_JWKS_URL`)
- Transport: `Authorization: Bearer <token>`
- Apple Sign-In: required for iOS App Store compliance

## IAP Security
- **Never trust client-side** IAP receipts
- Server-side validation via Stripe (for subscription management)
- Stripe webhook: `STRIPE_WEBHOOK_SECRET` for event validation

## IAM Roles
| Role | Access |
|------|--------|
| `user` | Own habits and logs only |
| `admin` | All data via `/api/v1/admin/*` |
| Stripe webhook | Payment events only |

## Secrets
| Secret | Storage |
|--------|---------|
| `NEON_AUTH_JWKS_URL` | Render env |
| `DB_URL` | Render env |
| `STRIPE_SECRET_KEY` | Render env |
| `STRIPE_WEBHOOK_SECRET` | Render env |
| `STRIPE_PREMIUM_PRICE_ID` | Render env |
| `ALLOWED_ORIGINS` | Render env |
