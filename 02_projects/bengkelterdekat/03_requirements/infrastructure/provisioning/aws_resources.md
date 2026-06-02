---
tags: [provisioning, infrastructure]
---

# Provisioning — Bengkelterdekat

## Hosting Strategy
| Env | Backend | Frontend | DB |
|-----|---------|----------|----|
| Dev | Local Go + `air` hot reload | `npm run dev` | Local PostgreSQL |
| Staging | Railway | Vercel Preview | Supabase staging |
| Prod | Railway / AWS ECS | Vercel | Supabase production |

## Local Dev (Makefile)
```bash
make dev     # hot reload via air
make run     # go run ./cmd/api
make test    # go test -race -cover ./...
make lint    # golangci-lint run
make build   # outputs bin/api
```

## DB Migrations
- Tool: `golang-migrate` SQL files
- Run: `make migrate-up DB_URL=...`

## Supabase Setup
- Region: Singapore
- Auth: email + Google OAuth (admin only)
- Storage: `workshop-photos` bucket (public read, admin write)
