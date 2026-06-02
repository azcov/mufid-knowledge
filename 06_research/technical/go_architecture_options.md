---
tags: [research, golang, architecture, microservices]
source: claude-convertation
date: 2026-05-31
---

# Go Architecture Options

Source: "Golang microservices framework options" (2026-05-31) + "Golang web directory examples" (2026-05-24)

## Microservices Frameworks

### Full Frameworks
| Framework | Stars | Style | Best For |
|-----------|-------|-------|---------|
| **go-kit/kit** | ~26K | Explicit Transport→Endpoint→Service, zero magic | Teams wanting full control, verbose but predictable |
| **go-kratos/kratos** | ~23K | API-first, Protobuf, Bilibili's cloud-native | High-performance API services |
| **cloudwego/kitex** | ~7K | ByteDance's RPC, 30K+ services at ByteDance | High-throughput RPC microservices |
| **go-micro/go-micro** | ~21K | Pluggable microservices toolkit | Plugin-based architecture |
| **nats-io** | — | Messaging/pubsub | Event-driven services |

### Cloud Portability / Infra Abstraction (gokit category)
| Library | Purpose |
|---------|---------|
| **google/go-cloud** | Write once, swap providers (S3/GCS/Azure Blob, SQL, pubsub) |
| **gokit (azcov)** | Internal abstraction layer — AI, cache, DB, email, storage, payments, etc. |

**Key insight from research:** The pattern `google/go-cloud` follows is exactly what `gokit` implements internally — interface-first, provider-swappable, no lock-in.

## Architecture Decision: Monolith-First → Microservices
Current approach across all labs projects:
- **Phase 1:** DDD monolith (handler → usecase → repository layering)
- **Phase 2:** Extract high-load services if needed
- No need to over-engineer from day 1

## Monorepo vs Separate Repos
Source: "Monorepo vs separate repositories" (2026-05-04)

### For Go + React/Vue — Recommendation: **Monorepo** (early stage)

| Aspect | Monorepo | Separate Repos |
|--------|----------|----------------|
| PRs | One PR covers FE + BE | Separate PRs |
| CI/CD | Single pipeline | Multiple pipelines |
| Local dev | Simpler setup | More configuration |
| Team access control | Harder | Easier |
| Split later | Easy (a few hours) | N/A |

**When to split:** When FE and BE teams work fully independently, or deployment cadences diverge significantly. Splitting is straightforward — move folders, update CI/CD pipelines.

### Suggested Monorepo Structure (Go + React)
```
my-app/
├── backend/          # Go
│   ├── cmd/api/
│   ├── internal/
│   │   ├── handler/
│   │   ├── usecase/
│   │   └── repository/
│   └── pkg/
├── frontend/         # Next.js / React
├── mobile/           # React Native (Expo)
└── docs/
```
