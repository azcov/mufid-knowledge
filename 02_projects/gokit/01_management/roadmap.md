---
tags: [management, roadmap]
---

# Roadmap — gokit

## Goal
Production-grade Go toolkit — implement all categories with popular providers, minimum 40% test coverage.

## Implementation Status (from 2026-06-01 session)

### Done ✅
- AI category: OpenAI, Anthropic, OpenRouter, Ollama providers
- Analytics: PostHog
- Auth: JWT, Clerk, Supabase
- Cache: Memory, Redis, Ristretto, Dragonfly
- Config: Env, .env, YAML, JSON
- Cron: robfig/cron, gocron
- DB: PostgreSQL, MySQL, MongoDB, CockroachDB
- Email: SMTP, Resend, SendGrid
- Errorz: structured errors with Is/As/Wrap/Log
- Geo: Nominatim (OpenStreetMap)
- HTTP: Retry client, graceful-shutdown server
- Lock: Memory, Redis
- Logger: slog, Uber Zap
- Middleware: CORS, rate limit, recovery, auth, request-id, timeout, logger, gzip
- Monitoring: OpenTelemetry, Prometheus, StatsD
- Payment: Stripe, PayPal, Midtrans, Xendit, Razorpay, DOKU
- PubSub: Memory, Kafka, NSQ
- Queue: Asynq (enqueuer + worker)
- Response: JSON envelope helpers
- Storage: S3, GCS, R2, Local
- Tracking: Sentry
- Tracing: OpenTelemetry + additional providers
- Utils: Generics, pagination, date/time/UUID
- Vector: pgvector

### In Progress 🔧
- Adding more providers per category (tracing: Datadog, Jaeger, Zipkin in addition to OTel)
- Test coverage to ≥ 40%
- Go upgraded to 1.26

## Quality Bar
- Production-grade code (not stubs)
- Each category has a minimal interface + at least 2 concrete providers
- Tests use httptest mocks (no live external calls)
- Context propagation on all methods
- No global state, no init() side effects
