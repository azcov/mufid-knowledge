---
tags: [security, auth, iam]
---

# Security & Auth

## Auth Strategy

Suratpedia has two tiers of access: public (no auth required) and admin (JWT required).

### Public access
- All `GET /api/categories`, `GET /api/templates`, `GET /api/seo`, `GET /api/templates/search` endpoints are unauthenticated
- `POST /api/download` is unauthenticated — any user can trigger file generation
- No end-user account system; no registration, no user-level sessions

### Admin access
- Single admin credential stored in environment variables (`ADMIN_EMAIL`, `ADMIN_PASSWORD`)
- Login via `POST /api/admin/login` with email + password
- On success: server sets a **JWT token in an httpOnly cookie** named `admin_token`
- Cookie attributes: `HttpOnly` (not readable by JavaScript), `Secure` (HTTPS only in production), `SameSite=Strict`
- All `/api/admin/*` routes are protected by a JWT middleware that validates the cookie
- No multi-admin or role-based access; single operator model

### Token validation
- JWT is signed with `JWT_SECRET` environment variable (minimum 32 characters recommended)
- Standard claims; expiry not documented but implied by JWT standard practice
- Frontend auth guard: each admin Client Component calls a protected API in `useEffect`; on `401 UNAUTHORIZED` it redirects to `/admin/login`

## Secrets Management

All secrets are passed via environment variables. No secrets manager (Vault, AWS Secrets Manager) is documented.

| Variable | Secret type | Where used |
|----------|------------|------------|
| `JWT_SECRET` | Signing key | Backend — sign and verify admin JWT |
| `ADMIN_PASSWORD` | Admin credential | Backend — compared at login |
| `REVALIDATE_SECRET` | Shared secret | Frontend + Backend — authenticate ISR revalidation calls |
| `R2_ACCESS_KEY` | API key | Backend — Cloudflare R2 object storage auth |
| `R2_SECRET_KEY` | API secret | Backend — Cloudflare R2 object storage auth |

Production checklist items related to secrets:
- Replace all default secrets before going live
- Set `ENV=production` in backend so cookie `Secure` flag is active
- Do not commit `.env` to version control

## IAM / Roles

| Role | Access | Notes |
|------|--------|-------|
| Anonymous user | All public GET endpoints, POST /api/download | No authentication required |
| Admin | All public endpoints + all /api/admin/* endpoints | Single credential from env vars; JWT cookie session |
| Parser service | Internal only — called by backend via HTTP | No auth on parser service; relies on Docker network isolation |
| Backend (for R2) | R2 Object Read & Write on `suratpedia-storage` bucket | Cloudflare R2 API token scoped to specific bucket |

## Firewall / Network Rules

- Parser service (port 8001) should not be exposed to the public internet; it is internal to the Docker Compose network only
- PostgreSQL (5432) and Redis (6379) should not be bound to public interfaces in production
- Backend CORS must be configured to allow only `suratpedia.com` in production (currently permissive in development)
- Cloudflare R2 bucket requires public read access for the generated download URLs to work; write access is restricted to the backend API token
- Production: set `CORS_ORIGIN=https://suratpedia.com` in backend environment (noted in deployment checklist)
