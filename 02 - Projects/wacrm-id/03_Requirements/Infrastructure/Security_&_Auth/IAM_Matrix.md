---
tags: [security]
---
# Security & Auth — WaCRM

## Auth
Supabase Auth (email + Google OAuth). JWT HS256 in Go middleware.

## Multi-Tenancy
All data scoped to `workspace_id`. Every query filters by workspace.
Team members can only see contacts assigned to their workspace.

## Tiers
Freemium limits enforced server-side (50 contacts free, 3 tags free, no reminders free).
