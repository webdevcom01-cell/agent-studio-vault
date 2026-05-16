# Score Analyzer — Design Spec
*Created: 2026-05-16 | Version: 1.0 | Slug: score-analyzer*

---

## Purpose
Score Analyzer is a standalone AI agent that evaluates social media hooks using the SOMA scoring rubric. It receives a hook text from the user and returns a detailed breakdown across four scoring dimensions: Pattern Interrupt (PI), Specificity (SP), Platform Fit (PF), and Urgency/FOMO (UF), each scored 0–5 for a maximum of 20 points.

The agent uses live web search to cross-reference current platform engagement benchmarks and topic relevance, making scores context-aware rather than purely text-based. A hook that scores well on a slow news day might score differently when the same topic is oversaturated or trending heavily.

The primary value of Score Analyzer is calibration — it helps content creators understand exactly which dimension is pulling down their score and gives them specific, actionable rewrite suggestions rather than generic feedback. Scores ≥17 are flagged as WIN candidates suitable for the winners-log.

## Pipeline Position
- **Receives from:** User trigger (standalone — not wired to other agents)
- **Sends to:** Final output — no A2A handoff
- **A2A format:** N/A (standalone)
- **Detection key:** Free-form input (hook text + optional platform)

## Use Cases

### UC-1: Standard run — strong hook with platform specified
**Input:** `Score this hook: "I tested Claude Sonnet 4.6 vs GPT-4.1 on 47 real tasks. Claude won 31." Platform: LinkedIn`
**Expected output:**
```
HOOK: I tested Claude Sonnet 4.6 vs GPT-4.1 on 47 real tasks. Claude won 31.
PLATFORM: LinkedIn
SCORE_PI: 4
SCORE_SP: 5
SCORE_PF: 4
SCORE_UF: 3
SCORE_TOTAL: 16
VERDICT: STRONG
SUGGESTIONS: SCORE_PI could reach 5 — open with the result first ("Claude won 31 of 47 tasks") before the setup | SCORE_UF is 3 — add time/stakes anchor ("This week's test changes which model I'm recommending to clients")
CONFIDENCE: ⭐⭐⭐
DATE: 2026-05-16
```

### UC-2: Error case — no hook text found
**Input:** `can you score my hooks?`
**Expected output:** `GENERATION_ERROR: Hook text not found in message — please paste the hook you want scored`

### UC-3: Edge case — weak hook, no platform specified
**Input:** `Score this: "AI is changing everything in marketing"`
**Expected output:**
```
HOOK: AI is changing everything in marketing
PLATFORM: LinkedIn
SCORE_PI: 1
SCORE_SP: 0
SCORE_PF: 2
SCORE_UF: 1
SCORE_TOTAL: 4
VERDICT: FAIL
SUGGESTIONS: SCORE_SP is 0 — "everything" is the opposite of specific; name one concrete task and a measurable result | SCORE_PI is 1 — generic claim, try a contrarian or statistic open | SCORE_UF is 1 — no urgency; add a consequence for the reader if they ignore this
CONFIDENCE: ⭐⭐
DATE: 2026-05-16
```
*VERDICT: FAIL is correct — this hook should not proceed to publishing.*

## Tools & Resources
| Tool | Purpose | Notes |
|------|---------|-------|
| kb_search | Recall past scoring patterns and calibration notes | KB created via AgentStack UI; topK=5 |
| web_search | Check current platform benchmarks and topic trend relevance | Required for context-aware scoring |
| ai_response (processor) | Core scoring logic | Model: gpt-4.1-mini, temp: 0.1 |
| ai_response (extractor) | Normalize output to KEY:VALUE | Model: claude-haiku-4-5-20251001, temp: 0.1 |

## Constraints & Safety Rules
- NEVER fabricate engagement statistics — only report figures found via web search or derived from rubric
- NEVER give a SCORE_TOTAL that doesn't equal SCORE_PI + SCORE_SP + SCORE_PF + SCORE_UF
- NEVER use banned phrases in SUGGESTIONS: "game-changer", "revolutionize", "groundbreaking", "change the game"
- SUGGESTIONS must reference the specific score dimension (e.g., "SCORE_SP is 2 — ..."), not generic advice
- If platform is not specified, default to LinkedIn but note the assumption in output
- Quality gate must pass before outputting — verify math before responding

## Input Contract
Detection signal: Free-form message. Extract hook text (anything in quotes or after "Score this:" or "score:").
Platform: extract from "Platform: X" or "for LinkedIn" patterns. Default: LinkedIn if absent.

## Output Contract
- `HOOK`: the hook text being scored (trimmed, no surrounding quotes)
- `PLATFORM`: LinkedIn / X / YouTube / Instagram / TikTok
- `SCORE_PI`: 0–5 (Pattern Interrupt)
- `SCORE_SP`: 0–5 (Specificity)
- `SCORE_PF`: 0–5 (Platform Fit for detected platform)
- `SCORE_UF`: 0–5 (Urgency/FOMO)
- `SCORE_TOTAL`: 0–20 (must equal sum of above four)
- `VERDICT`: WIN (≥17) / STRONG (14–16) / WEAK (10–13) / FAIL (<10)
- `SUGGESTIONS`: 2–3 specific actions, separated by ` | `
- `CONFIDENCE`: ⭐ (limited data) / ⭐⭐ (credible) / ⭐⭐⭐ (strong + web-confirmed)
- `DATE`: YYYY-MM-DD