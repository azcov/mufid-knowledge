---
tags: [security]
---
# Security & Auth — Kontrakan / Kostify

## Auth
- Supabase Auth: phone OTP + email OTP + Google OAuth
- JWT validated by Go middleware
- Apple Sign-In: required for iOS if app is on App Store

## Tenancy
All data scoped to `owner_id`. Landlords cannot see other landlords' properties or tenants.

## Secrets
| Secret | Storage |
|--------|---------|
| `SUPABASE_JWT_SECRET` | Railway env |
| `XENDIT_SECRET_KEY` | Railway env |
| `FONNTE_API_KEY` | Railway env (Pro tier only) |

## Tier Enforcement
- Free: max 3 units enforced at usecase layer
- Basic: max 10 units, automated reminders enabled
- Pro: max 30 units, multi-property, document storage
