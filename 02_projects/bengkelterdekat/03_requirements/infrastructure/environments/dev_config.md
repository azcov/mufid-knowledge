---
tags: [environment, dev]
---

# Dev Environment — Bengkelterdekat

## Setup
```bash
# Backend
cd backend
cp .env.example .env
make dev  # hot reload via air, port 8080

# Frontend
cd frontend
npm install
npm run dev  # port 3000
```

## Ports
| Service | Port |
|---------|------|
| Go API | 8080 |
| Next.js | 3000 |
| PostgreSQL | 5432 (local Docker or Supabase dev) |
