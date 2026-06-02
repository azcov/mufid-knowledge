---
tags: [requirements, architecture]
---
# Technical Architecture — gokit

## Package Structure
```
gokit/
├── ai/          — AI provider interface + OpenAI, Anthropic, Ollama
├── auth/        — Auth provider interface + JWT, Supabase, Clerk
├── cache/       — Cache interface + Redis, Memory, Ristretto
├── config/      — Config source interface + env, yaml, json
├── db/          — DB interface + PostgreSQL, MySQL, MongoDB
├── email/       — Mailer interface + SMTP, Resend, SendGrid
├── http/        — HTTP client (retry) + server (graceful shutdown)
├── middleware/  — Chi/Gin-compatible middleware collection
├── payment/     — Payment gateway interface + Xendit, Stripe, Midtrans
├── storage/     — Storage interface + S3, GCS, R2, Local
├── tracing/     — Tracer interface + OTel, Datadog, Jaeger
└── utils/       — Generics, pagination, UUID helpers
```

## Usage Pattern (e.g., AI)
```go
import (
    "github.com/azcov/gokit/ai"
    "github.com/azcov/gokit/ai/openai"
)

var provider ai.Provider = openai.New(apiKey)
resp, err := provider.Complete(ctx, ai.Request{...})
```
Swap to Anthropic: `var provider ai.Provider = anthropic.New(apiKey)` — no other changes.

## Used By
All Mufid & Aziz Labs Go projects import gokit for common infrastructure.
