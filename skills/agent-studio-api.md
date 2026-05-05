---
title: "Agent Studio Api"
tags: ["skill", "auto-synced", "agent-studio", "api", "agents", "flows", "skills"]
updated: "2026-05-05T17:21:35.342Z"
---

# Agent Studio API

## Agenti

```typescript
// Lista agenata
GET /api/agents
Authorization: Bearer <token>

// Kreiranje agenta
POST /api/agents
{
  "name": "Support Agent",
  "systemPrompt": "Ti si asistent za podršku...",
  "model": "gpt-4o"
}
```

## Flows

```typescript
// Izvršavanje flow-a
POST /api/flows/{id}/execute
{
  "input": { "message": "Zdravo!" }
}
```

## ECC Skills

```typescript
// Ingest skills
POST /api/ecc/ingest-skills
Authorization: Bearer <cron_secret>
{
  "skills": [{ "slug": "...", "content": "..." }]
}

// Health check
GET /api/health
```