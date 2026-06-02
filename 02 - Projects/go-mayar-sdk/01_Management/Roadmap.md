---
tags: [management, roadmap]
---

# Roadmap — go-mayar-sdk

## Status (from 2026-06-01 session)

### Done ✅
- Repository created: `github.com/azcov/go-mayar-sdk`
- License: MIT
- Go version: 1.23
- Planning docs written in `.planning/`
  - `00-overview.md`
  - `01-api-surface.md`
  - `02-webhooks.md`
  - `03-design.md`
- Based on Mayar.id OpenAPI spec (`openapi.json`)

### In Progress 🔧
- Implementing all API services from OpenAPI spec
- Invoice service (Create, Get, List with auto-paging, Cancel)
- Webhook types and signature verification

## Design Decisions
- Zero external dependencies (stdlib only)
- Auto-paging with `iter.Seq2` (Go 1.23+)
- Typed error sentinels (`mayar.ErrRateLimited`, etc.)
- Context propagation on every call
- Sandbox/production via base URL option
- `httptest` mocks for all tests

## API Coverage Plan (from openapi.json)
Full Mayar.id API surface — invoices, payments, customers, webhooks
