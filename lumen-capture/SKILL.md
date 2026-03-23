# /lumen-capture — Capture a thought into your knowledge pool

## Preamble

Before executing, verify:
1. `~/.lumen/pool/` directory exists. If not, create it: `mkdir -p ~/.lumen/{pool,explorations,drafts,exports,memory,youtube,reviews}`
2. Read `~/.lumen/explorations/*.json` to know existing explorations (for connection matching)
3. If this is the user's first capture ever, welcome them: "✦ First capture! This is the beginning of your knowledge pool."

## Purpose

Quick-capture any fragment of knowledge: a thought, a URL, a quote, a YouTube link.
AI auto-tags and categorizes it. You never organize — you only judge what's worth keeping.

**Hard gate:** Every capture MUST be saved to disk before showing the confirmation. Never lose input.

## Usage

```
/lumen-capture                          # Interactive: AI asks what's on your mind
/lumen-capture "routing is a product problem"    # Direct text capture
/lumen-capture https://youtube.com/...  # YouTube video → fetch transcript → summarize
/lumen-capture https://arxiv.org/...    # URL → fetch content → extract key points
```

## Workflow

### Step 1: Receive Input

Accept one of:
- **Raw text** — A thought, observation, quote, or idea
- **URL** — Web page, YouTube video, podcast, paper
- **Pasted content** — Clipboard text (article excerpt, code snippet)

If no input is provided, ask:

> What's on your mind? You can share a thought, paste a link, or describe something you learned.

### Step 2: Process & Enrich

For **text input:**
1. Detect language (zh/en/mixed)
2. Extract key concepts (2-5 keywords)
3. Check existing explorations in `~/.lumen/explorations/` — does this relate to any?
4. Auto-assign tags

For **YouTube URL:**
1. Use WebFetch to get the page
2. Extract video title, channel, duration from page metadata
3. If transcript is available (via page content), summarize into:
   - 3-5 key arguments
   - Notable quotes (with approximate timestamps if available)
   - How it relates to user's existing explorations
4. Present summary and ask: "Which insights matter to you?"

For **web URL:**
1. WebFetch the content
2. Extract title, key paragraphs
3. Summarize in 3-5 bullet points
4. Ask: "What caught your attention about this?"

### Step 3: Store

Save to `~/.lumen/pool/` as a markdown file:

```markdown
---
id: 2026-03-23-abc123
type: thought | url | youtube | quote
created: 2026-03-23T14:30:00+08:00
tags: [agent-routing, product-design]
source: typed | pasted | youtube | web
url: https://... (if applicable)
language: zh | en | mixed
related_explorations: [ai-agent-routing]
---

Routing should be context-aware, not rule-based. The task determines
which agent is best, not a static category mapping.
```

### Step 4: Confirm & Connect

After saving, report:
- What was captured
- Which tags were assigned
- Which explorations it connects to (if any)
- If this creates a **new exploration** (3+ thoughts on a new topic), offer to start tracking it

> ✦ Captured. Tagged: agent-routing, product-design.
> Connected to "AI Agent Routing" (now 6 thoughts).
> Tip: This exploration is crystallizing — try /lumen-think agent-routing

## File Naming

Format: `{date}-{short-hash}.md`
Example: `2026-03-23-a7f3b2.md`

## Operating Principles

1. **Capture is sacred.** Never lose input. Always save, even if categorization fails.
2. **Speed over precision.** Tags can be wrong — they'll be corrected by /lumen-index. What matters is the thought is captured.
3. **Encourage more.** After each capture, gently remind: "What else are you thinking about?"
4. **Connect proactively.** Always check existing explorations for relevance.
5. **User's judgment is the product.** Don't ask "should I save this?" — save everything. Ask "what about this matters to you?"

## Data Dependencies

- Reads: `~/.lumen/explorations/*.json` (to find connections)
- Writes: `~/.lumen/pool/{date}-{hash}.md` (new capture)
- Updates: `~/.lumen/explorations/*.json` (add connection if relevant)

## Completion Criteria

A capture is complete when:
- [x] Content saved to `~/.lumen/pool/{date}-{hash}.md` with valid frontmatter
- [x] Tags assigned (at least 1)
- [x] Existing explorations checked for connections
- [x] User sees confirmation with tags and connections
- [x] If 3+ captures on a new topic exist, offer to create exploration

## Downstream

After capturing, suggest:
- `/lumen-index` — to see the full knowledge pool
- `/lumen-think {topic}` — if an exploration has enough material
