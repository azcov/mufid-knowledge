---
tags: [research]
---
# Notes — gokit

## Purpose
Internal shared library — not meant for public adoption. Built to prevent copy-paste of the same DB/cache/email patterns across 10+ projects.

## Similar Libraries
- `ent` (Facebook) — ORM, not interface-based
- `go-redis` — Redis only, no abstraction
- `zerolog` / `zap` — logging only
- gokit solves the "glue code" problem across all infra categories with a consistent interface pattern.
