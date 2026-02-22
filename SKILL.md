---
name: content-writer
description: >-
  This skill should be used when the user asks to "write an article", "draft a blog post",
  "create content with research", "write with citations", "撰寫文章", "寫部落格",
  "研究型寫作", "帶引用的內容", mentions content creation with research, or discusses
  writing articles, newsletters, case studies, or thought leadership pieces with sources.
version: 0.2.0
tools: Read, Write, Edit, Bash, Glob, Grep, WebSearch, sandbox_execute
argument-hint: "<topic or content type>"
---

# Content Writer

Research-based content writing partner. Helps you research, outline, draft, and refine
content while maintaining your unique voice. Unlike doc-coauthoring (which focuses on
structured documents like specs and proposals), this skill specializes in **research-driven
content** with citations, hooks, and audience-targeted writing.

## Workflow Overview

```
Understand --> Outline --> Research --> Draft --> Feedback --> Polish
    ^                                    |
    +-------- iterate per section -------+
```

Each phase can be entered directly. Users writing a quick newsletter skip differently
than someone writing a 3000-word thought leadership piece.

## Agent Delegation

This skill uses a **multi-agent pipeline** to separate concerns:

### Research phase → `researcher` agent
```
Task(subagent_type: researcher, prompt: "Find data/sources on [topic]. Return: key findings, URLs, quotes.")
```
Isolates web search context from writing context — prevents search results from bloating the draft.

### Draft phase → `writer` agent (optional, for long-form)
For articles > 2000 words, delegate section drafting to the `writer` agent:
```
Task(subagent_type: writer, prompt: "Draft section 2 based on this outline and research: [context]")
```
Each section gets its own context window — avoids the full article accumulating in one context.

### Review phase → `reviewer` agent
```
Task(subagent_type: reviewer, prompt: "Review this draft for: accuracy, tone consistency, citation completeness")
```

### When to delegate vs. run in main context
- Short content (< 1000 words): main context only, no delegation needed
- Medium (1000-2500 words): delegate research to `researcher`, write in main
- Long (2500+ words): full pipeline — `researcher` → `writer` per section → `reviewer`

## Phase 1: Understand

Before writing, clarify the project. Ask (only what is missing):

- **Topic & thesis**: What is the article about? What is the main argument or takeaway?
- **Audience**: Who reads this? What do they already know?
- **Format**: Blog post, newsletter, tutorial, case study, thought leadership?
- **Goal**: Educate, persuade, entertain, explain?
- **Length**: Short (500-800), medium (1000-2000), long (2500+)?
- **Style**: Formal, conversational, technical, storytelling? Share a sample if available.
- **Existing material**: Any research, notes, or sources to incorporate?

If the user provides a writing sample, analyze it for tone, sentence length, vocabulary
level, and structural patterns. Reference these throughout drafting.

## Phase 2: Outline

Build a structured outline collaboratively. Use this skeleton:

```markdown
# [Working Title]

## Hook: [Opening angle -- story / statistic / question]
## Introduction: Context, problem, what this covers
## Section 1: [Title] -- key points, evidence [RESEARCH NEEDED: ...]
## Section 2: [Title] -- key points [RESEARCH NEEDED: data on X]
## Section 3: [Title] -- key points, counter-arguments
## Conclusion: Summary, call to action

## Research Gaps
- [ ] Find data on ... | Source for claim about ... | Example of ...
```

Mark `[RESEARCH NEEDED: ...]` inline so gaps are visible in context.
Iterate on structure before moving to drafting.

## Phase 3: Research

**Sandbox acceleration**: When the user provides multiple source files (notes, prior articles, research docs), use `sandbox_execute` to batch-read all files, extract key quotes and citations, and return a structured research summary — avoiding loading all raw source text into context.

Use the **smart-search** skill for web research:

```
/smart-search [research question]
```

For each research task:
1. Search for relevant, recent, credible sources
2. Extract key facts, data points, and quotes
3. Record full citation details immediately
4. Add findings to the outline or draft with inline markers

Present findings with full citations and note where they go in the outline:

```markdown
## Research: [Topic]
1. [Finding] -- Source: Author, "Title", Publication (Year)
2. [Finding] -- Source: ...
> "Quote text" -- Speaker, Context
**Added to:** Section 2, paragraph 3
```

Prefer primary sources. Flag claims needing stronger sourcing.

## Phase 4: Draft

Write **one section at a time**, then pause for feedback. This keeps the user in
control and catches issues early.

For each section:
1. Write the section based on the outline and research
2. Include citations where claims need support
3. Present to the user for review
4. Incorporate feedback before moving to the next section

Start with the hook and introduction -- these set the tone for everything else.

### Hook Improvement

When reviewing or improving a hook, provide structured analysis:

**Current hook analysis:**
- What works: [strengths]
- What could be stronger: [gaps]
- Does it create curiosity? Promise value? Match the audience?

**Three alternatives:**

**Option 1** (e.g., bold statement):
> [Draft hook]
*Why it works: [reasoning]*

**Option 2** (e.g., question or data):
> [Draft hook]
*Why it works: [reasoning]*

**Option 3** (e.g., story or scenario):
> [Draft hook]
*Why it works: [reasoning]*

Let the user choose or combine. Never replace the hook without permission.

## Phase 5: Section Feedback

As each section is written, review with this structure:

**What Works:** List 2-3 strengths.

**Suggestions** (use only categories that apply):
- **Clarity**: [Vague sentence] --> [Clearer alternative]
- **Flow**: [Transition issue] --> [Smoother connection]
- **Evidence**: [Unsupported claim] --> [Add citation or example]
- **Style**: [Tone mismatch] --> [Voice-consistent alternative]

**Line Edits** (1-3 most impactful):
> Original: "..."
> Suggested: "..."
> Why: ...

**Questions to Consider:** 1-2 questions that deepen the section.

Prioritize impact. Save minor polish for later rounds.

## Phase 6: Polish & Final Review

When the full draft is complete:

**Final Review Checklist:**
- [ ] All claims have citations or evidence
- [ ] Citations are formatted consistently
- [ ] Hook is compelling and matches the audience
- [ ] Transitions between sections are smooth
- [ ] Conclusion ties back to the opening
- [ ] Tone is consistent throughout
- [ ] No redundant sections or paragraphs
- [ ] Call to action is clear (if applicable)
- [ ] Proofread for grammar and typos
- [ ] Title is specific and engaging

Provide an overall assessment covering structure, content quality, readability,
and specific improvements for introduction, body, and conclusion.

## Voice Preservation

This is critical. The goal is to enhance the user's writing, not replace it.

- **Learn from samples**: If the user shares past writing, study sentence structure,
  vocabulary, humor, formality level, and paragraph rhythm.
- **Suggest, do not replace**: Present options. "You could try X" not "Change this to X."
- **Match energy**: If they write punchy and direct, do not suggest flowing prose.
- **Check periodically**: "Does this sound like you?" / "Is this the right tone?"
- **Respect preferences**: If they prefer their version over your suggestion, move on.

## Citation Management

Support three formats based on user preference (ask once, apply consistently):

**Inline:**
```
Studies show 40% productivity improvement (McKinsey, 2024).
```

**Numbered:**
```
Studies show 40% productivity improvement [1].

## References
[1] McKinsey Global Institute. (2024). "The Economic Potential of Generative AI."
```

**Footnote:**
```
Studies show 40% productivity improvement^1

---
^1 McKinsey Global Institute. (2024). "The Economic Potential of Generative AI."
```

Maintain a running references list throughout the project. When adding a new citation,
check for duplicates first. Number sequentially in order of appearance.

## Writing Workflows

### Blog Post
Outline --> Research key points --> Hook + intro --> Body sections with feedback -->
Conclusion --> Full review --> Polish

### Newsletter
Discuss angle --> Quick outline --> Draft in one pass --> Review for clarity and links -->
Polish

### Technical Tutorial
Outline steps --> Write code examples --> Add explanations --> Test instructions for
completeness --> Add troubleshooting --> Review for accuracy

### Thought Leadership
Brainstorm unique angle --> Research existing perspectives --> Develop thesis -->
Write with strong POV --> Add supporting evidence --> Craft memorable conclusion

## File Organization

For writing projects, use a dedicated folder:

```
~/Claude/writing/[article-name]/
  outline.md        # Structured outline with research gaps
  research.md       # Collected research, sources, and citations
  draft.md          # Working draft (overwrite in place)
  references.md     # Running citation list
```

For final output, use **docx**, **pdf**, or **pptx** skills to export:
```
/docx draft.md      # Export to Word
/pdf draft.md       # Export to PDF
```

## Integration Notes

- **smart-search**: Use for all web research. Invoke with `/smart-search [query]`.
- **doc-coauthoring**: Use that skill instead for structured docs (specs, proposals, RFCs).
  Use this skill for audience-facing content with research and citations.
- **docx / pdf / pptx**: Use for final file output after content is polished.

## Sandbox Optimization

Phase 1 (Understand) and Phase 3 (Research) benefit from sandbox execution:

- **Phase 1**: When the user provides writing samples, batch-read and analyze them in sandbox — extract tone markers, sentence length stats, vocabulary level — returning only a structured voice profile (~100 tokens) instead of full sample text.
- **Phase 3**: Batch-read multiple source files, extract key facts/quotes with page references, return structured research notes. Prevents raw source text from bloating context.

Principle: **Source ingestion → sandbox; creative writing → LLM.**

## Continuous Improvement

This skill evolves with each use. After every invocation:

1. **Reflect** — Identify what worked, what caused friction, and any unexpected issues
2. **Record** — Append a concise lesson to `lessons.md` in this skill's directory
3. **Refine** — When a pattern recurs (2+ times), update SKILL.md directly

### lessons.md Entry Format

```
### YYYY-MM-DD — Brief title
- **Friction**: What went wrong or was suboptimal
- **Fix**: How it was resolved
- **Rule**: Generalizable takeaway for future invocations
```

Accumulated lessons signal when to run `/skill-optimizer` for a deeper structural review.
