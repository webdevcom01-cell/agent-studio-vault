---
tags:
  - agent
  - score-analyzer
  - evo-log
  - soma
created: "2026-05-16"
modified: "2026-05-15"
---

# Score Analyzer — Evolution Log
*Path: agents/score-analyzer/evo-log*

---

## Log Format
Each entry appended after a run:
```
date | hook_preview | score_total | verdict | confidence | notes
```

---

## Instincts Update Trigger
After **20+ entries**: increase topK from 5 → 15 in `kb_search-score-analyzer-memory`.
After **5 entries with QUALITY_GATE_FAIL**: update instincts.md with new failure pattern.

---

## Entries

*No entries yet. Agent created 2026-05-16. Smoke test pending KB creation.*


2026-05-16 | "I went from 0 to 47K followers in 90 days using one scheduling trick" | 17 | WIN | ⭐⭐⭐ | UC-1 smoke test. All quality gates passed. Math correct (4+4+5+4=17). SUGGESTIONS referenced correct dimension names. Note: extractor model patched to gpt-4.1-mini (ANTHROPIC_API_KEY not set on server).