---
tags: [security, auth]
---
# Security & Auth — IndoDev-ID

## Auth
- Supabase Auth (email + Google OAuth)
- JWT HS256 validated by Go middleware

## IAM Roles
| Role | Access |
|------|--------|
| Developer | Own profile, applications only |
| Company admin | Own company, job posts, applicants only |
| Platform admin | All data via `/api/v1/admin/*` |
| Xendit webhook | Boost payment events only |

## Rules
- Company can only see applicants to their own posts
- Developer apply rate / status visible to developer only
- Salary range: enforced at usecase layer (not just frontend)
