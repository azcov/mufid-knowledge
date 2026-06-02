---
tags: [requirements, specs]
---
# Functional Specs — gokit

## Categories & Providers

| Category | Interface | Providers |
|----------|-----------|-----------|
| AI | `Provider` | OpenAI, Anthropic, OpenRouter, Ollama |
| Analytics | `Tracker` | PostHog |
| Auth | `Provider` | JWT, Clerk, Supabase |
| Cache | `Cache` | Memory, Redis, Ristretto, Dragonfly |
| Config | `Source` | Env, .env, YAML, JSON |
| Cron | `Scheduler` | robfig/cron, gocron |
| DB | `DB` | PostgreSQL, MySQL, MongoDB, CockroachDB |
| Email | `Mailer` | SMTP, Resend, SendGrid |
| Geo | `Geocoder` | Nominatim (OSM) |
| HTTP | `Client` | Retry client, graceful-shutdown server |
| Lock | `Lock` | Memory, Redis |
| Logger | `Logger` | slog, Uber Zap |
| Middleware | `Middleware` | CORS, rate limit, recovery, auth, request-id, timeout, logger, gzip |
| Monitoring | `Metrics` | OpenTelemetry, Prometheus, StatsD |
| Payment | `Gateway` | Stripe, PayPal, Midtrans, Xendit, Razorpay, DOKU |
| PubSub | `PubSub` | Memory, Kafka, NSQ |
| Queue | `Enqueuer/Worker` | Asynq |
| Storage | `Storage` | S3, GCS, R2, Local |
| Tracking | `Tracker` | Sentry |
| Tracing | `Tracer` | OpenTelemetry, Datadog, Jaeger, Zipkin |
| Utils | — | Generics, pagination, date/time/UUID |
| Vector | `Store` | pgvector |
