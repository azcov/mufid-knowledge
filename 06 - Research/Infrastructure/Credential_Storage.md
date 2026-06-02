---
tags: [research, credentials, secrets, security, infrastructure]
source: claude-convertation
date: 2026-05-26
---

# Where to Store App Credentials

Source: "Tempat penyimpanan kredensial aplikasi" (2026-05-26)

## Per Environment

### Local Development
- `.env` file — never commit to git
- `.env.example` — commit this (with placeholder values)
- Use `direnv` for auto-loading per project directory

### Staging / Production (Server)
| Option | Notes |
|--------|-------|
| **Railway env vars** | Set in Railway dashboard, injected at runtime |
| **Render env vars** | Same pattern |
| **Server `.env` file** | For Oracle VM / Hetzner VPS — store at `/opt/appname/.env`, restricted permissions (`chmod 600`) |
| **Docker secrets** | For Docker Swarm/Compose production |

### Cloud Secrets Managers (when scaling)
| Service | Cost | Notes |
|---------|------|-------|
| AWS Secrets Manager | $0.40/secret/month | Best for AWS-native stacks |
| HashiCorp Vault | Free self-hosted | Complex but powerful |
| Doppler | Free (25 secrets) | Developer-friendly, all environments |
| **Infisical** | Free open source | Best free option, self-hostable |

## Recommended Pattern for Solo Dev
1. **Dev**: `.env` file (gitignored), `.env.example` in repo
2. **Prod**: Railway/Render env vars dashboard OR server `/opt/app/.env` (chmod 600)
3. **Never**: hardcode in code, commit to git, log to console

## For Claude Code / AI Agents
- Use `.claude/settings.local.json` for project-specific permissions (not secrets)
- API keys go in `.env`, not in CLAUDE.md or settings files

## Tools Used Across Projects
- `golang-dotenv` or `os.Getenv()` for Go
- `process.env` for Node/Next.js
- All secrets stored in Railway env vars for backend services
