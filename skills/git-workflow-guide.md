---
title: "Git Workflow Guide"
tags: ["skill", "auto-synced", "git", "version-control", "workflow", "collaboration"]
updated: "2026-05-05T14:14:00.815Z"
---

# Git Workflow

## Branch strategija

```bash
# Feature branch
git checkout -b feature/user-auth

# Commit sa konvencijom
git commit -m "feat(auth): add JWT token refresh logic"

# Push i PR
git push origin feature/user-auth
```

## Commit konvencije (Conventional Commits)

- `feat:` nova funkcionalnost
- `fix:` ispravka greške
- `docs:` dokumentacija
- `refactor:` refaktoring bez promjene funkcionalnosti
- `test:` dodavanje testova
- `chore:` build/tooling izmjene

## Rebase umjesto merge

```bash
# Ažuriranje feature brancha
git checkout feature/user-auth
git rebase main

# Interaktivni rebase za čišćenje
git rebase -i HEAD~3
```