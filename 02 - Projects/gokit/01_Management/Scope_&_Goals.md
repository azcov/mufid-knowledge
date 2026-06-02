---
tags: [management, scope]
---
# Scope & Goals — gokit

## Goal
Production-grade Go toolkit. Clean abstraction interfaces + concrete provider implementations for common backend infrastructure. One interface, many providers. Swap without changing application code.

## Install
```bash
go get github.com/azcov/gokit@latest
```

## Design Principles
- Interface-first: every category defines a minimal Go interface
- Zero lock-in: switch Redis → Dragonfly, Sentry → Rollbar with one line
- No magic: no global state, no init() side effects, explicit construction
- Production-ready: structured errors, context propagation, graceful shutdown, retry logic
