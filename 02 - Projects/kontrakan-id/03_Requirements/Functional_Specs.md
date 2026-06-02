---
tags: [requirements, specs]
---
# Functional Specs — Kontrakan / Kostify

## Core Features

### Property + Unit Management
- Create properties, add units per property
- Unit: name/number, type (kamar/unit), rent amount, payment period (monthly/quarterly/semi-annual)

### Tenant & Contract
- Tenant profile: name, phone, KTP (optional)
- Contract: tenant + unit + start date + end date + rent amount
- Auto-generates payment cycles from contract (monthly → 12 cycles, quarterly → 4, etc.)

### Payment Dashboard
- See all units: paid / late / upcoming due
- Mark payment received → cycle updates to paid
- Late payment: auto-flagged by background scheduler

### WhatsApp Reminders
- **Manual (all tiers):** Generate wa.me link with pre-filled reminder message → landlord taps to send
- **Automated (Pro only):** Fonnte gateway sends message 3 days before due, on due date, 3 days after

### Contract Expiry Alerts
- 60 days and 30 days before expiry → alert to landlord
- Time to renegotiate or find new tenant

### Monthly Income Report
- Expected income vs actual received per unit
- Occupancy rate, late count
- Pro: exportable

## WhatsApp Reminder Flow (Auto)
```
Scheduler (every 1 hour)
  → Query cycles: status=pending/late, due soon, Pro tier owners
  → POST Fonnte API: send message to tenant phone
  → On success: log sent
  → On failure: fall back to wa.me for manual send
```

## Key Data Model
- `properties` → `units` → `contracts` → `payment_cycles`
- `whatsapp_logs` — every reminder attempt (sent_via: gateway | wa_me)
- `tenants` — per owner (not shared)
