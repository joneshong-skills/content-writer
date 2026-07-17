---
name: content-writer
description: "content, writer, write, article, draft, blog, post, 撰寫文章, 寫部落格, 研究型寫作, 帶引用的內容"
version: 0.2.0
tools: Read, Write, Edit, Bash, Glob, Grep, WebSearch, sandbox_execute
argument-hint: "<topic or content type>"
---

# Content Writer

## Phase 1: Understand

Resolve these REQUIRED points before proceeding. Points already stated in — or reasonably inferable from — the initial request count as resolved; do not re-ask them. Ask only about genuinely unresolved points, batched into ONE message, each with a best-guess default the user can accept or override:

1. **Topic & thesis** — Main subject and central argument
2. **Audience** — Reader profile and knowledge level
3. **Format** — Blog post, newsletter, tutorial, case study
4. **Goal** — Educate, persuade, entertain, or explain
5. **Length** — Short (500–800), medium (1000–2000), or long (2500+)
6. **Style** — Formal, conversational, technical, or storytelling

**Optional:** Writing sample to match voice

**Stop condition:** All 6 REQUIRED points resolved (stated, inferred, or defaulted after one batched ask). When the user requests output only, resolve every point by inference or default, list them under "Assumptions:", and proceed directly — never substitute questions for the deliverable.

## Output Format

Deliver final content as:
- Markdown file (`.md`) with consistent formatting
- Bibliography or reference list if using numbered or footnote citations

## Phase 2: Outline

Produce a markdown outline with:
- Working title
- Hook (opening angle)
- Introduction (context and scope)
- 2–4 body sections, each labeled `[RESEARCH NEEDED: <specific gap>]`
- Counter-arguments or alternative views (if applicable)
- Conclusion (summary and call to action)
- Checklist of all `[RESEARCH NEEDED: ...]` gaps

**Stop condition:** All gaps indexed and listed. Do not proceed to Phase 3 if gaps are vague or missing.

## Phase 3: Research

For each research gap:
1. Run `/smart-search [specific research question]`
2. Record finding in this format:
   - **Fact:** [statement]
   - **Source:** Author, "Title", Publication (Year)
   - **Quote:** "[direct quote]" — [speaker/context]
   - **Section:** [outline section name]
3. Mark gaps as either "FILLED" or "NO RELIABLE SOURCE FOUND"

**Stop condition:** Every gap marked FILLED or NO SOURCE FOUND. Unfilled gaps block Phase 4.

## Phase 4: Draft

1. **Select citation format** — Inline, numbered, or footnote. Lock this choice for the entire piece.
2. **Draft order:** Hook → Introduction → Section 1 → Section 2 → Conclusion
3. **Per section:**
   - Write from outline and research notes with citations placed at claim points
   - Present for user review
   - Revise based on feedback before moving to next section

**Stop condition:** All sections drafted and user-approved. No section may skip to Phase 5 without approval.

## Phase 5: Section Feedback

Apply one feedback pass per section. Address:
- **Clarity** — Rephrase unclear statements; add concrete examples
- **Flow** — Improve transitions between ideas
- **Evidence** — Add citations where missing; remove if >1 per paragraph
- **Style** — Match tone to writing sample
- **Line edits** — Most impactful phrasing improvements

**Structural changes** (new sections, outline rewrites, removed content) return to Phase 2, not Phase 5.

**Stop condition:** Section passes review and user explicitly approves before Phase 6.

## Phase 6: Polish & Final Review

Verify checklist:
- [ ] All claims have citations
- [ ] Citations formatted consistently
- [ ] Hook is compelling for target audience
- [ ] Transitions smooth between sections
- [ ] Conclusion ties back to opening
- [ ] Tone consistent throughout
- [ ] No redundant or filler content
- [ ] Call to action clear
- [ ] Grammar correct
- [ ] Title is specific and engaging

**If any item fails:** Return to its originating phase:
- **Unsourced claims** → Phase 3 (research)
- **Weak draft sections** → Phase 4 (redraft)
- **Structure problems** → Phase 2 (outline)
- **Tone/flow/line issues** → Phase 5 (feedback)
- **Grammar/formatting** → Phase 5 (line edits)

**Stop condition:** All checkboxes pass.