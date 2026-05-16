# Content Repurposer — Instincts
*Path: /agents/content-repurposer/instincts*
*Last updated: 2026-05-15*

---

## Learned Patterns

### Platform adaptation principles

**LinkedIn → the piece with the most technical depth**
- Developer audience expects substance, not just a hook
- Structure that works: Hook → 3 insights → 1 contrarian → 1 specific question
- Insight blocks: 3-4 sentences each, one concrete example per block
- CTA questions that drive comments: "How are you handling X in your current stack?" beats "What do you think?"
- Word count: shoot for 500-600 words — under 300 feels thin, over 800 loses the feed scroll

**X Thread → the fastest, densest piece**
- Each tweet must stand alone as a complete thought
- Thread narrative: Problem → Context → Implication → Mistake → Fix → Takeaway → CTA
- Never split a sentence across tweets — complete thought per tweet
- Don't pad to 6 tweets if 5 is cleaner

**YouTube → the most educational piece**
- Script outline, not full script — enough for a confident presenter to riff
- Thumbnail must promise something specific and deliverable in the video length
- Section titles: outcome-oriented ("How to X" not "About X")
- Tags: mix exact-match terms with broad discovery terms — 8-10 total
- HOOK line: 15 words max, end on a half-thought to create watch pressure

**Instagram → the most visual piece**
- Carousel cover (Slide 1) must work as a standalone billboard
- Each slide: one insight only — resist the urge to stack points
- Supporting lines: concrete, not abstract — "3 nodes, not 300 lines of code" not "simplified architecture"
- CTA slide should ask a question that invites saves ("Save this if you're building X")
- Hashtags: 8-12 is optimal; >15 looks spammy to developer audience

**TikTok → the most raw piece**
- Write it as if you're talking to a developer friend, not an audience
- Each point: 8-12 words max — TikTok rewards density
- PAYOFF must deliver something surprising or counter-intuitive
- No corporate language anywhere — "leverage" and "utilize" are instant credibility killers
- CTA: "Follow for X" works better than "Comment below" for dev content

### Cross-platform adaptation rules
- Each platform receives its own pre-optimized hook via payload (HOOK_LINKEDIN, HOOK_X, HOOK_YOUTUBE, HOOK_INSTAGRAM, HOOK_TIKTOK) — use them directly, do not re-select or substitute
- LinkedIn body ≠ thread tweets — completely different content, same topic
- TikTok script should feel like the LinkedIn post's rebellious younger sibling
- Never copy-paste a sentence from one platform piece into another

### Developer voice test (applied before finalizing each piece)
Ask: "Could this sentence appear in a B2C product blog or general tech newsletter?"
If YES → rewrite for builders. The audience knows what APIs, prompts, and tokens are.

BAD: "This feature streamlines AI integration for consistent outputs."
GOOD: "Set once in system prompt — no per-request config, no repeated context overhead."

BAD: "Improve user experience with dynamic adjustments."  
GOOD: "Swap instruction sets per user segment without touching your app logic."

BAD: "Leverage this powerful tool to enhance your workflow."
GOOD: "One kb_search node, injected via {{kb_context}} — agents read it on every run."

### Common mistakes
- DO NOT generate a "YouTube full script" — outline only, sections + bullets
- DO NOT use same hook text across platforms — each platform has its own hook from payload
- Word count check: if LinkedIn draft < 300 words, it's not done — expand the body
- TikTok SPOKEN hook: count the words manually before finalizing. Max 12. If >12, cut.

---

## Quality Gate Failures (learned from runs)

### 🔴 Run 3 — 2026-05-15 — Banned phrase slipped through on LinkedIn Line 2
**What happened:** LinkedIn hook Line 1 was clean (P1 pattern, no emojis, ✅). But Line 2 contained "change the game" — a banned phrase — despite it being explicitly listed in hard_constraints.

**Root cause:** The quality_gate scans the entire output but Line 2 of the hook sits right at the top before the body. Easy to miss when attention is on the main content block.

**Fix:** Before writing LinkedIn Line 2 — do a banned phrase spot-check on that line specifically, not just as part of the full output scan. "Change the game" and all variants are the most likely to slip in as natural filler on Line 2.

**Banned Line 2 patterns to actively avoid:**
- ❌ "...change the game" / "...changes everything" / "...is a game-changer"
- ❌ "...revolutionizes how we build" / "...transforms your workflow"
- ✅ "...cuts deployment time from days to hours" (specific + measurable)
- ✅ "...and most builders haven't noticed yet" (stakes without banned words)
- ✅ "...without touching your orchestration layer 🔧" (technical specificity)

---

### 🔴 Run 3 — 2026-05-15 — Fabricated statistic in LinkedIn body
**What happened:** LinkedIn body opened with "68% of AI agents still need manual tweaking" — a statistic that did not appear anywhere in the incoming payload or KB context. Violated the NO FABRICATED STATISTICS hard constraint.

**Root cause:** P1 hook pattern naturally invites a percentage ("X% of developers still..."). When no real stat exists in the payload, the model filled it in from training data.

**Fix:** If the incoming HOOK_LINKEDIN is a P1 (Hard Stat) pattern, check whether the stat is actually in the payload before using it in the body. If not there → do NOT use it. Instead, reframe using a concrete feature description.

**Safe alternatives when no stat is available:**
- ❌ "68% of AI agents still need manual tweaking" (invented)
- ✅ "Most AI agent teams report the same bottleneck: manual tuning at every deployment cycle."
- ✅ "The pattern is consistent across teams building with [TREND]: manual intervention is still the default."
- ✅ "Walk into any agent-building team and you'll find the same problem: [describe without % claim]"
