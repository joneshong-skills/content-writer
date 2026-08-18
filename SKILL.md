---
name: content-writer
description: "content, writer, write, article, draft, blog, post, 撰寫文章, 寫部落格, 研究型寫作, 帶引用的內容"
version: 0.2.0
tools: Read, Write, Edit, Bash, Glob, Grep, WebSearch, sandbox_execute
argument-hint: "<topic or content type>"
---

# Content Writer

## Output Format
Deliver final content as Markdown (`.md`) with bibliography if using citations.

## Phase 1: Understand

Resolve these 6 REQUIRED points:

1. **Topic & thesis** — Main subject and central argument
2. **Audience** — Reader profile and knowledge level
3. **Format** — Blog post, newsletter, tutorial, case study
4. **Goal** — Educate, persuade, entertain, or explain
5. **Length** — Short (500–800), medium (1000–2000), or long (2500+)
6. **Style** — Formal, conversational, technical, or storytelling

**If user specifies fewer than 6 points:** Infer what the request implies; mark those resolved. Ask only the genuinely unresolved points, batched into a single message, each with a best-guess default the user can accept or override.

**Stop condition — if user requests output only** (「直接給」「只輸出結果」"just write it", "output only", or similar)**:** Infer all 6 points (or apply defaults: general audience, blog post, 1500–2000 words, conversational), list them under `Assumptions:`, and proceed directly to Phase 2 — never substitute questions for the deliverable.

**Otherwise:** All 6 REQUIRED points resolved → proceed to Phase 2.

## Phase 2: Outline

Produce markdown outline:
- Working title, hook, introduction
- 2–4 body sections; mark each with `[RESEARCH NEEDED: <gap>]` only if sourcing required
- Counter-arguments or alternatives
- Conclusion with call to action
- **Final checklist:** Enumerate all `[RESEARCH NEEDED: ...]` tags with their section locations

**If checklist is empty (no gaps):** Skip Phase 3, proceed to Phase 4 (Draft)

## Phase 3: Research

For each `[RESEARCH NEEDED: ...]` gap:

1. Run `/smart-search [research question]`
2. Record findings:
   - **Fact:** [statement]
   - **Source:** Author, "Title", Publication (Year)
   - **Quote:** "[direct quote]" — [context]
   - **Section:** [outline section this fills]
3. Mark gap as **FILLED** or **UNFILLED**

**Source priority:** Peer-reviewed papers > official docs > authoritative experts > recent articles. Blog posts only if no better source; note limitation explicitly.

**When gaps remain unfilled:**
- If gap is non-critical (nice-to-have detail): Reframe that section in outline to omit it, return to Phase 2
- If gap is critical (core claim): Flag for user — proceed to Phase 4 with `[SOURCE PENDING]` placeholder, escalate to Phase 6 review

## Phase 4: Draft

1. **Confirm citation format** (inline, numbered, or footnote)
2. Write sequentially: Hook → Introduction → Section 1 → Section 2 → Conclusion
3. **Place citations at each factual claim** using research findings; limit 1 citation per paragraph
4. **Present full draft to user for approval** before proceeding to feedback

## Phase 5: Section Feedback

Apply feedback pass addressing:
- **Clarity** — Rephrase unclear statements; add examples
- **Flow** — Improve transitions
- **Evidence** — Add citations where missing; max 1 per paragraph
- **Style** — Match tone to audience and provided sample (if any)
- **Line edits** — Strongest phrasing improvements only

**Return to earlier phase if:**
- Major claims lack sources → Phase 3 (Research)
- Weak sections or outline mismatch → Phase 2 (Outline)
- Grammar, tone, or line-level phrasing → Revise in this phase, no reroute

## Phase 6: Polish & Final Review

Verify:
- [ ] All factual claims have citations
- [ ] All citations formatted consistently
- [ ] Hook engages target audience
- [ ] Transitions flow smoothly
- [ ] Conclusion ties back to opening
- [ ] Tone consistent throughout
- [ ] No filler or redundant content
- [ ] Call to action is clear
- [ ] Grammar and spelling correct
- [ ] Title is specific and engaging

**Route back if checklist items fail:**
- Unsourced claims → Phase 3
- Weak draft sections → Phase 4
- Structure problems → Phase 2
- Tone, flow, phrasing → Phase 5
- Grammar or formatting → Phase 5

## Output
Return the polished Markdown file.