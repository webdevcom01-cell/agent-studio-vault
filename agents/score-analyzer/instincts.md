---
tags:
  - agent
  - score-analyzer
  - instincts
  - soma
created: "2026-05-16"
version: "1.0"
---

# Score Analyzer — Instincts
*Learned patterns and calibration notes. Seed into KB. Update after every 5 runs.*

---

## Scoring Calibration

### SCORE_PI — Pattern Interrupt
- **5/5 triggers**: Counter-intuitive numbers ("I quit LinkedIn and got 3x more clients"), confession openers ("I was wrong about..."), specific shocking outcomes
- **Common 3/5 trap**: Starting with "How I..." — mildly interesting but overused on LinkedIn
- **Watch for**: Questions as openers score 2–3 on LinkedIn (overused), but score 4–5 on X where they drive replies

### SCORE_SP — Specificity
- **5/5 requires ALL THREE**: specific number + named tool/person + measurable outcome
- **Example 5/5**: "Used Taplio (tool) to schedule 3 posts/week (number) → 47K followers in 90 days (outcome)"
- **Common mistake**: Percentages without baseline = only 3/5. "47% increase" needs a starting number to hit 5/5.
- **"One trick/hack/strategy"** → automatic SP penalty: -1 (vague noun)

### SCORE_PF — Platform Fit
- **LinkedIn norms**: Professional tone, story arc preferred, max ~220 chars for hook, career/business topics
- **X norms**: Punchy, conversational, 140 chars ideal, hot takes score higher, memes/slang acceptable
- **YouTube norms**: Curiosity gap essential, "how" and "why" openers strong, longer hook OK (read as title)
- **Instagram norms**: Visual cue in hook preferred ("Look at this..."), lifestyle framing
- **TikTok norms**: Gen-Z casual, trending audio references, very short hook (<10 words ideal)

### SCORE_UF — Urgency/FOMO
- **5/5 triggers**: Time constraint ("...and it ends this week"), fear-of-missing-out ("everyone doing X but not you"), social proof gap ("3,000 people learned this before you")
- **LinkedIn FOMO**: Career advancement and professional gap work better than monetary FOMO
- **Watch for**: Fake urgency ("this changes everything") = 1/5. Specific urgency ("hiring closes Friday") = 4–5/5

---

## Quality Gate Rules (Non-Negotiable)
1. Math must be exact: SCORE_TOTAL = PI + SP + PF + UF. If not, output QUALITY_GATE_FAIL.
2. All scores must be integers 0–5. Decimals = QUALITY_GATE_FAIL.
3. SUGGESTIONS must name the dimension: "SCORE_SP is 2 — ..." not "Add specificity"
4. Banned phrases in SUGGESTIONS: game-changer, revolutionize, groundbreaking, change the game
5. VERDICT thresholds are hard: WIN=≥17, STRONG=14–16, WEAK=10–13, FAIL=<10. No rounding up.

---

## Platform Default Logic
If no platform mentioned in input:
- Professional language + career topic → LinkedIn
- Short + punchy + slang → X
- "video" mentioned → YouTube
- Visual language + lifestyle → Instagram
- "viral" + very short → TikTok
- Fallback: **LinkedIn**

---

## Confidence Rating Guide
- ⭐⭐⭐ High: Hook text is clear, platform is specified, web search found current benchmarks
- ⭐⭐ Medium: Platform inferred OR web search results sparse OR hook text ambiguous
- ⭐ Low: Hook text unclear, no platform context, web search returned nothing relevant

---

## Common Edge Cases
| Situation | Handling |
|-----------|----------|
| Hook in non-English | Score as-is, note language in SUGGESTIONS if PF is affected |
| Multiple hooks in one message | Score only the first hook; mention others were skipped |
| Hook is a question | Often PI=3 on LinkedIn (overused), PI=4 on X (drives replies) |
| Hook mentions competitor brand | Score normally; do not editorialize about brands |
| Hook contains profanity | Score normally for platform fit; X may benefit, LinkedIn penalizes |
| No hook found | Output: GENERATION_ERROR: No hook text found — please paste the hook directly |

---

## Changelog
| Date | Change | Trigger |
|------|--------|---------|
| 2026-05-16 | v1.0 — Initial instincts created at scaffold | Agent creation |
