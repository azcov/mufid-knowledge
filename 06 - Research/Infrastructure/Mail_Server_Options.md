---
tags: [research, email, mail-server, infrastructure]
source: claude-convertation
date: 2026-05-27
---

# Mail Server Options for Custom Domain

Source: "Solusi mail server untuk domain" (2026-05-27)

## Terminology
- **Email Hosting / Mail Hosting** = layanan yang handle send + receive email dari custom domain (misal `kamu@domainmu.com`)
- **Web Hosting** ≠ Email Hosting (dua layanan terpisah)
- **Domain** = alamat saja, tidak otomatis dapat email

## Free Options

| Layanan | Bisa Kirim? | Bisa Terima? | Limit | Catatan |
|---------|------------|--------------|-------|---------|
| **Zoho Mail Free** | ✅ | ✅ | 5 user, 5GB/user | Terbaik untuk $0 — tapi hanya via webmail/Zoho app (no IMAP/POP) |
| **Cloudflare Email Routing** | ❌ (forward ke Gmail) | ✅ | Unlimited alias | Zero setup, unlimited forwarding |
| **ImprovMX** | ❌ (forward saja) | ✅ | 1 domain, 25 alias | Alternatif Cloudflare |

**Rekomendasi gratis:** Cloudflare Email Routing (forward ke Gmail) + Zoho untuk reply dari domain.

## Under $1/Month Options

| Layanan | Harga | IMAP/POP | Notes |
|---------|-------|----------|-------|
| **Truehost** | ~$0.40/mo | ✅ | Paling murah, reliable |
| **Namecheap Private Email** | ~$0.91/mo | ✅ | Populer, stabil |

## Affordable Paid Options

| Layanan | Harga | Notes |
|---------|-------|-------|
| **Migadu** | $4/mo | Unlimited domain, unlimited user |
| **Zoho Mail Lite** | $1/user/mo | IMAP support, affordable |
| **Google Workspace** | $6/user/mo | Paling nyaman jika sudah pakai Google ecosystem |
| **Fastmail** | $3/mo | Privacy-focused |

## Recommendation per Use Case
- **Solo founder, testing, baru mulai:** Cloudflare forward → Gmail (forward in, reply via Gmail with alias)
- **Perlu kirim + terima, budget $0:** Zoho Mail Free (5 user, webmail only)
- **Perlu IMAP, budget minimum:** Truehost ($0.40/mo) atau Namecheap ($0.91/mo)
- **Multi-domain, growing:** Migadu ($4/mo, unlimited domain)
- **Full Google ecosystem:** Google Workspace ($6/user/mo)
