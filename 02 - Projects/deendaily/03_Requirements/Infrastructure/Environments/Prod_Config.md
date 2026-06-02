---
tags: [environment, prod]
---

# Production Environment — DeenDaily

## Hosting
| Service | Platform |
|---------|----------|
| Go API | Render.com (Singapore, Starter plan) |
| Mobile | App Store + Google Play Store |
| Database | Neon (production branch) |

## Deploy
- Backend: auto-deploy on push to `main` via Render.com
- Mobile: EAS Build → manual submit to App Store Connect / Play Console

## Pre-deploy
1. All tests pass
2. DB migrations applied (`golang-migrate`)
3. All env vars set in Render dashboard
4. Health check `/health` returns 200
