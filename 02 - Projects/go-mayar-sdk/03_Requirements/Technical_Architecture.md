---
tags: [requirements, architecture]
---
# Technical Architecture — go-mayar-sdk

## Package Structure
```
go-mayar-sdk/
├── mayar.go         — Client struct, constructor, options
├── invoice.go       — InvoiceService
├── payment.go       — PaymentService
├── errors.go        — Typed error types
├── paginator.go     — Auto-paging iterator (iter.Seq2)
└── *_test.go        — httptest mocks per service
```

## Design Decisions
- No ORM, no framework — pure stdlib `net/http`
- `iter.Seq2` (Go 1.23+) for lazy pagination
- All API calls accept `context.Context` (respects deadlines/cancellation)
- Sandbox/production via base URL option — no global state
