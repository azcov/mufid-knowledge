---
tags: [requirements, specs]
---
# Functional Specs — go-mayar-sdk

## Services Wrapped
- `client.Invoice` — create, get, list (auto-paging), cancel
- `client.Payment` — payment status, payment links
- `client.Customer` — customer management

## Quick Start
```go
client := mayar.New(apiKey, mayar.WithSandbox())
invoice, err := client.Invoice.Create(ctx, mayar.CreateInvoiceParams{
    Name: "John Doe", Email: "john@example.com",
    Items: []mayar.InvoiceItem{{Quantity: 1, Rate: 50000, Description: "Item A"}},
})
```

## Client Options
```go
mayar.New(apiKey)                    // production (api.mayar.id)
mayar.New(apiKey, mayar.WithSandbox()) // sandbox (api.mayar.club)
```

## Error Handling
Typed sentinel errors:
- `mayar.ErrRateLimited`
- `mayar.ErrUnauthorized`
- `mayar.ErrNotFound`
