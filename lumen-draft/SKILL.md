# /lumen-draft — Generate a draft from accumulated thoughts

## Preamble

Before executing, verify:
1. Target exploration exists in `~/.lumen/explorations/`
2. Exploration has at least 3 captures (otherwise suggest `/lumen-capture` or `/lumen-think` first)
3. Load style memory from `~/.lumen/memory/style-{lang}.md` if it exists
4. Load think history for suggested outlines
5. Create `~/.lumen/drafts/` if it doesn't exist

**Hard gate:** The draft MUST primarily use the user's own words from their captures. AI connects and expands, but the voice and claims must come from the user's actual thoughts. NEVER generate claims the user didn't make.

## Purpose

Turn a crystallized exploration into a coherent first draft. Uses your accumulated captures, think session insights, and style memory to write in YOUR voice.

## Usage

```
/lumen-draft                        # AI picks the most ready exploration
/lumen-draft agent-routing          # Draft from specific exploration
/lumen-draft agent-routing --brief  # Short format (3-5 paragraphs)
/lumen-draft agent-routing --thread # X thread format directly
```

## Workflow

### Step 1: Load Exploration

Read from `~/.lumen/explorations/{topic}.json` + all linked captures + think history.
Read style memory from `~/.lumen/memory/style-{lang}.md`.

If no exploration specified, pick the one with status "Ready" or "Crystallizing" with most captures.

### Step 2: Confirm Angle & Structure

Present the suggested outline (from /lumen-think if available, or generate new):

> Your exploration "AI Agent Routing" has 8 thoughts and a think session.
>
> Suggested angle: **"I know my routing is wrong, and I ship it anyway"**
> (Build log — honest, personal, technical)
>
> Structure:
> 1. Hook — Everyone starts with if-else routing
> 2. The incident — Shuri and the cross-category task
> 3. Why I shipped it anyway — pragmatism over perfection
> 4. What context-aware routing might look like
> 5. The roadmap — rules → heuristics → ML
>
> Language: English (your captures are mostly EN on this topic)
>
> Does this angle work? Or adjust?

Use AskUserQuestion. Accept adjustments before drafting.

### Step 3: Generate Draft

Write the draft using:
- User's actual captures as source material (quote and expand, don't fabricate)
- Style memory for voice, tone, rhythm
- Think session insights for structure
- The user's own words as much as possible — AI fills gaps and connects

**Important:** The draft must feel like the user wrote it, not like AI generated it.

### Step 4: Present & Iterate

Show the draft in a readable format. Then ask:

> Here's your first draft (487 words).
>
> [Draft content...]
>
> What would you like to do?
> 1. **Use this** — Save to ~/.lumen/drafts/
> 2. **Adjust tone** — More casual / more technical / more personal
> 3. **Expand section** — Which section needs more depth?
> 4. **Try different angle** — Start over with a new approach
> 5. **Polish** → /lumen-polish

### Step 5: Save

Save to `~/.lumen/drafts/{topic}-draft-{n}.md` with frontmatter:

```markdown
---
exploration: ai-agent-routing
version: 1
language: en
word_count: 487
created: 2026-03-23T16:00:00+08:00
angle: "I know my routing is wrong, and I ship it anyway"
format: article
---

[Draft content]
```

Update exploration status to "Published" (or "Draft" if user wants to iterate).

## Operating Principles

1. **User's words first.** Use their captures as the raw material. AI connects and expands, not replaces.
2. **Voice must match.** Read style memory. If the user writes casually, don't produce formal prose.
3. **Never fabricate claims.** Only include facts/examples from the user's own captures.
4. **Show sources.** Mark which captures contributed to each section (as comments or footnotes).
5. **Draft ≠ final.** Set expectations that this is a starting point, not a finished piece.

## Data Dependencies

- Reads: `~/.lumen/explorations/{topic}.json`, `~/.lumen/pool/*.md`, `~/.lumen/memory/style-*.md`, think history
- Writes: `~/.lumen/drafts/{topic}-draft-{n}.md`
- Updates: `~/.lumen/explorations/{topic}.json` (status)

## Downstream

- Polish → `/lumen-polish {topic}`
- Export to platform format → `/lumen-export {topic}`
