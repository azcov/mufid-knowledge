---
tags: [environment, staging]
---

# Staging Environment

## URL

Not documented. No dedicated staging environment is described in the planning docs. The project uses a single Docker Compose setup that serves both development and production roles.

## Env Vars

Not documented as a separate staging configuration. If a staging environment is created, it should differ from dev/prod in the following ways:

| Variable | Staging recommendation |
|----------|----------------------|
| `NEXT_PUBLIC_API_URL` | URL of staging backend (e.g. `https://api-staging.suratpedia.com`) |
| `R2_BUCKET` | Separate R2 bucket (e.g. `suratpedia-storage-staging`) to avoid mixing with production data |
| `R2_PUBLIC_URL` | Separate custom domain for staging bucket |
| `ADMIN_EMAIL` / `ADMIN_PASSWORD` | Different credentials from production |
| `JWT_SECRET` | Different secret from production |
| `REVALIDATE_SECRET` | Different secret from production |
| `ENV` | `staging` (the backend checks `ENV=production` to enable Secure cookie flag) |

## Deploy Process

Not documented. The project's deployment mechanism is Docker Compose on a VPS. A staging deploy would follow the same steps as production:

```bash
docker-compose pull       # or rebuild images
docker-compose up -d
```

No CI/CD pipeline is documented. No staging-specific tests or smoke tests are documented.
