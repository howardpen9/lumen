# /lumen-polish — Refine a draft in your voice

## Preamble

Before executing, verify:
1. At least one draft exists in `~/.lumen/drafts/`. If not: "No drafts yet. Try `/lumen-draft` first."
2. Load style memory from `~/.lumen/memory/style-{lang}.md` if it exists
3. If no style memory exists, this is fine — we'll create one from the draft

**Hard gate:** Never overwrite the original draft. Always save as a new polished version. The user must be able to compare before/after.

## Purpose

Polish a draft for language quality, rhythm, and voice consistency. Supports separate Chinese and English style profiles.

## Usage

```
/lumen-polish                           # Polish most recent draft
/lumen-polish agent-routing             # Polish specific exploration's draft
/lumen-polish agent-routing --zh        # Polish / translate to Chinese
/lumen-polish agent-routing --en        # Polish in English
```

## Workflow

### Step 1: Load Draft & Style Memory

Read the latest draft from `~/.lumen/drafts/{topic}-draft-*.md`.
Read style memory from `~/.lumen/memory/style-{lang}.md`.

If style memory doesn't exist yet, analyze the draft to create initial style profile.

### Step 2: Polish Pass

Apply style rules:
- **Vocabulary**: Use preferred words, avoid disliked ones
- **Rhythm**: Match sentence length patterns (short-long-short)
- **Tone**: Match formality level (casual/professional/mixed)
- **Structure**: Match paragraph length, use of lists, headers

For Chinese: respect CJK typography, avoid stiff translation tone.
For English: match the user's natural voice, not "AI voice."

### Step 3: Show Diff

Present the polished version with changes highlighted. Ask:

> Polished version (12 changes):
> [Content with changes marked]
>
> Accept all / Review one by one / Adjust style?

### Step 4: Update Style Memory

After user accepts, analyze the final text and update style memory:

```markdown
<!-- ~/.lumen/memory/style-en.md -->
## Voice Profile (English)
- Tone: Casual-professional, conversational
- Sentence rhythm: Short declarative, then longer explanatory
- Vocabulary: Prefers "ship" over "deploy", "break" over "fail"
- Structure: Opens with anecdote, uses numbered lists for technical points
- Avoids: Corporate jargon, excessive hedging, emoji in body text
- Signature patterns: "Here's the thing though:" as pivot phrase

Last updated: 2026-03-23 (from "AI Agent Routing" draft)
```

## Data Dependencies

- Reads: `~/.lumen/drafts/*.md`, `~/.lumen/memory/style-*.md`
- Writes: `~/.lumen/drafts/{topic}-polished-{lang}.md`
- Updates: `~/.lumen/memory/style-{lang}.md`

## Downstream

- Export → `/lumen-export`
