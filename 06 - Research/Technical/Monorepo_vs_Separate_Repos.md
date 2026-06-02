---
tags: [research, architecture, monorepo, golang]
source: claude-convertation
date: 2026-05-04
---

# Monorepo vs Separate Repos — Go + React/Vue

Source: "Monorepo vs separate repositories for web app" (2026-05-04)

## Recommendation for Solo Go Developer: Monorepo ✅

Go and JS/TS don't share code natively (no shared types like Node monorepos), but monorepo still wins at early stage because:
- One place for issues, PRs, and docs
- Easier to keep FE/BE in sync during early development
- Simpler local dev setup

## Comparison

| Aspect | Monorepo | Separate Repos |
|--------|----------|----------------|
| PRs | One PR covers FE + BE changes | Separate PRs per repo |
| CI/CD | Single pipeline | Multiple pipelines to maintain |
| Local dev | Simpler one-command setup | More config files |
| Team access control | Harder | Per-repo permissions |
| Repo size | Grows large | Stays focused |
| Split later | Easy (a few hours) | N/A |

## Recommended Structure
```
my-app/
├── backend/          # Go
│   ├── cmd/api/main.go
│   ├── internal/
│   │   ├── handler/
│   │   ├── usecase/
│   │   └── repository/
│   └── pkg/
├── frontend/         # Next.js / React
├── mobile/           # React Native (Expo)
├── docs/
└── .claude/
```

## Splitting Later
When to split: FE and BE teams work fully independently, or very different deployment cadences.
What's involved: Move folders to new repos, update CI/CD, update import paths. Takes a few hours.

## Applied In
All Mufid & Aziz Labs projects use this pattern:
- [[02 - Projects/kulitku/00_Project_Hub|Kulitku]] — backend + frontend + mobile + scraper
- [[02 - Projects/budgetnest/00_Project_Hub|BudgetNest]] — backend + frontend + mobile
- [[02 - Projects/kontrakan-id/00_Project_Hub|Kostify]] — backend + web + mobile (pnpm workspace)
