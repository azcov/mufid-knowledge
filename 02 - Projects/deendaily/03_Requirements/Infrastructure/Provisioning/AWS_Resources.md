---
tags: [provisioning, infrastructure]
---

# Provisioning — DeenDaily

## Hosting Strategy
| Env | Backend | Mobile | DB |
|-----|---------|--------|----|
| Dev | Docker Compose (local, port 8080) | Expo Go app | Neon (dev branch) |
| Staging | Render.com preview | Expo preview | Neon staging branch |
| Prod | Render.com Singapore (starter) | App Store + Play Store | Neon production |

## Render.com (render.yaml)
- Type: web service (Docker)
- Region: Singapore
- Plan: Starter
- Health check: `/health`
- Auto-deploy: on push to `main`

## Mobile Distribution
- iOS: App Store Connect → TestFlight (beta) → Production
- Android: Google Play Console → Internal testing → Production
- Build: EAS Build (Expo Application Services)

## Docker Compose (Local Dev)
```yaml
services:
  api: Go backend → port 8080 (uses external Neon DB)
  web: Next.js → port 3000 (post-MVP)
```
