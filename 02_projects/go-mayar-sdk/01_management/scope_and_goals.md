---
tags: [management, scope]
---
# Scope & Goals — go-mayar-sdk

## Goal
Idiomatic Go SDK for the Mayar.id payment and commerce platform. Used by other labs projects that need Mayar.id integration.

## Design Principles
- Zero dependencies (stdlib only — no transitive bloat)
- Auto-paging: `for inv := range client.Invoice.All(ctx, nil)` walks every page
- Typed errors: `errors.Is(err, mayar.ErrRateLimited)`
- Idiomatic: `context.Context` on every call, `iter.Seq2` for streaming, no magic globals
- Tested: `httptest` mocks for every service

## Install
```bash
go get github.com/azcov/go-mayar-sdk
```
