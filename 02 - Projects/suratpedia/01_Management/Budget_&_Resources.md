---
tags: [management, budget]
---

# Budget & Resources

## Infrastructure Costs

### Cloudflare R2 (primary storage)
- **Free tier**: 10 GB storage, 1 million Class A operations/month, 10 million Class B operations/month
- **Egress cost**: Free (no egress fees from R2 to the internet — this is a key reason R2 was chosen over S3)
- **Beyond free tier**: $0.015/GB/month storage, $4.50 per million Class A ops, $0.36 per million Class B ops
- **Bucket name**: `suratpedia-storage`
- **Custom domain**: `r2.suratpedia.com`

### Docker Compose (self-hosted VPS)
- All five services (frontend, backend, parser, postgres, redis) run on a single Docker Compose host
- No cloud-managed database or cache — cost is just the VPS
- VPS sizing not documented; parser image is ~500MB (requires LibreOffice), so the host needs at least 2GB RAM
- Recommended: add PostgreSQL volume backup to an external service (mentioned in prod checklist)

### Domain
- `suratpedia.com` — not documented where registered; assumed via a domain registrar

## Tools & Services

| Tool | Purpose | Cost |
|------|---------|------|
| Cloudflare R2 | File storage (DOCX, PDF, images) | Free tier / pay-as-you-go |
| Cloudflare CDN/Proxy | Frontend delivery (optional, mentioned in prod checklist) | Free tier available |
| PostgreSQL 16 | Primary database | Self-hosted in Docker |
| Redis 7 | Download URL cache | Self-hosted in Docker |
| Docker Compose | Container orchestration | Free / open-source |
| LibreOffice (in parser container) | DOCX-to-PDF conversion | Free / open-source |
| python-docx | DOCX parsing | Free / open-source |
| pdfplumber | PDF parsing | Free / open-source |

## Time Allocation

Not documented. Single-developer project (inferred from single admin credential design and Docker Compose rather than Kubernetes).

## Cost Optimization Notes

- **Tiered caching** is the primary cost control mechanism: files with fewer than 10 downloads get a 7-day TTL, preventing long-term accumulation of low-traffic files on R2.
- **Lazy generation** means no storage is used for templates that are never downloaded.
- **Nightly cron** at 03:00 actively deletes expired files from R2 and Redis to prevent unbounded storage growth.
- The planning docs specifically call out R2's free egress as "krusial untuk download file" — choosing R2 over S3 is a deliberate cost decision.
