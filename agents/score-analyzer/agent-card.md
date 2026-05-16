---
tags:
  - agent
  - score-analyzer
  - soma
  - scoring
created: "2026-05-16"
agent_id: cmp7gtng100hdpc01cqr5d2hy
---

# Score Analyzer — Agent Card

## Identity
| Field | Value |
|-------|-------|
| **Agent Name** | Score Analyzer |
| **Agent ID** | `cmp7gtng100hdpc01cqr5d2hy` |
| **Public URL** | https://agent-studio-production-c43e.up.railway.app/agents/cmp7gtng100hdpc01cqr5d2hy |
| **Model** | gpt-4.1-mini |
| **Role** | Standalone scorer |
| **Created** | 2026-05-16 |
| **Knowledge Base ID** | PENDING — create KB via UI, then patch `kb_search-score-analyzer-memory` |

---

## Pipeline Position
```
[User Input]
     │
     ▼
kb_search-score-analyzer-memory  (topK=5)
     │
     ▼
web_search-score-analyzer-live
     │
     ▼
ai_response-score-analyzer-processor  (gpt-4.1-mini, temp=0.1)
     │
     ▼
ai_response-score-analyzer-extractor  (claude-haiku-4-5-20251001, temp=0.1)
     │
     ▼
[structured_output]
```

**Standalone agent** — not in TI→HW→CR chain. Called directly or via future hook-writer integration.

---

## Input Contract
| Variable | Source | Description |
|----------|--------|-------------|
| `user_message` | User input | Hook text + optional platform name |
| `kb_context` | kb_search node | Scoring instincts, calibration notes, past patterns |
| `search_results` | web_search node | Current platform benchmarks, trending topics |

**Example input:**
```
Score this hook for LinkedIn: "I went from 0 to 47K followers in 90 days using one scheduling trick"
```

---

## Output Contract
All outputs via `structured_output` variable (KEY:VALUE plain text, A2A FORMAT C).

| Key | Values |
|-----|--------|
| `HOOK` | The hook text being scored |
| `PLATFORM` | LinkedIn / X / YouTube / Instagram / TikTok |
| `SCORE_PI` | 0–5 (Pattern Interrupt) |
| `SCORE_SP` | 0–5 (Specificity) |
| `SCORE_PF` | 0–5 (Platform Fit) |
| `SCORE_UF` | 0–5 (Urgency/FOMO) |
| `SCORE_TOTAL` | 0–20 (sum of above) |
| `VERDICT` | WIN (≥17) / STRONG (14–16) / WEAK (10–13) / FAIL (<10) |
| `SUGGESTIONS` | 2–3 actions separated by ` | ` |
| `CONFIDENCE` | ⭐ / ⭐⭐ / ⭐⭐⭐ |
| `DATE` | ISO date of scoring |

**Error codes:**
- `QUALITY_GATE_FAIL:` — math error or invalid scores
- `GENERATION_ERROR:` — hook text not found in message

---

## Flow Node IDs
| Node | ID |
|------|----|
| KB Search | `kb_search-score-analyzer-memory` |
| Web Search | `web_search-score-analyzer-live` |
| Processor | `ai_response-score-analyzer-processor` |
| Extractor | `ai_response-score-analyzer-extractor` |

---

## Vault Files
| File | Path |
|------|------|
| Agent Card (this file) | `agents/score-analyzer/agent-card.md` |
| Instincts | `agents/score-analyzer/instincts.md` |
| Evolution Log | `agents/score-analyzer/evo-log.md` |
| Design Spec | `agents/score-analyzer/DESIGN_SPEC.md` |

---

## KB Seeding Checklist
- [ ] Create KB via AgentStack UI → name: "Score Analyzer Memory"
- [ ] Run `as_list_knowledge_bases` to get KB ID
- [ ] Patch `kb_search-score-analyzer-memory` → set `knowledgeBaseId`
- [ ] Seed doc 1: `agents/score-analyzer/DESIGN_SPEC.md`
- [ ] Seed doc 2: `agents/score-analyzer/instincts.md`
- [ ] Seed doc 3: `agents/score-analyzer/evo-log.md` (first entry after smoke test)
