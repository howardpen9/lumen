# Lumen Data Format Specification

All data lives in `~/.lumen/`. This format is shared between V1 (CLI skills) and V2 (macOS app).

## Directory Structure

```
~/.lumen/
├── pool/                       # Raw captures
│   ├── 2026-03-23-a7f3b2.md
│   └── ...
├── explorations/               # AI-grouped topic clusters
│   ├── ai-agent-routing.json
│   ├── ai-agent-routing-think-history.json
│   └── ...
├── drafts/                     # Generated drafts
│   ├── ai-agent-routing-draft-1.md
│   ├── ai-agent-routing-polished-en.md
│   └── ...
├── exports/                    # Platform-formatted outputs
│   ├── ai-agent-routing-x-thread.md
│   └── ...
├── memory/                     # Style profiles + persona
│   ├── style-en.md
│   ├── style-zh.md
│   └── persona.json
├── youtube/                    # Video summary cache
│   ├── dQw4w9WgXcQ.json
│   └── ...
├── reviews/                    # Weekly review snapshots
│   ├── 2026-03-23-review.md
│   └── ...
└── config.json                 # User preferences
```

## Capture Format (`pool/*.md`)

```markdown
---
id: 2026-03-23-a7f3b2
type: thought | url | youtube | quote
created: 2026-03-23T14:30:00+08:00
tags: [agent-routing, product-design]
source: typed | pasted | youtube | web | think-session | apple-notes
url: https://... (optional)
language: zh | en | mixed
related_explorations: [ai-agent-routing]
video_title: "..." (youtube only)
channel: "..." (youtube only)
timestamp: "~1:24:30" (youtube only)
---

The actual captured content goes here.
Can be multiple paragraphs.
```

### ID Format
`{YYYY-MM-DD}-{6-char-hash}`
- Hash: first 6 chars of hex-encoded random bytes
- YouTube captures: `{YYYY-MM-DD}-yt-{6-char-hash}`

## Exploration Format (`explorations/*.json`)

```json
{
  "id": "ai-agent-routing",
  "title": "AI Agent Routing",
  "status": "seed | exploring | crystallizing | ready | draft | published",
  "captures": [
    "2026-03-19-abc123",
    "2026-03-20-def456"
  ],
  "tags": ["routing", "multi-agent", "product-design"],
  "created": "2026-03-19T10:00:00+08:00",
  "updated": "2026-03-23T14:30:00+08:00",
  "ai_summary": "Your evolving position: routing is a product design problem, not just an engineering challenge.",
  "connections": ["context-engineering"],
  "thesis": "Routing should be context-aware, not rule-based."
}
```

### Status Transitions
```
seed (1-2 captures)
  → exploring (3-5 captures)
    → crystallizing (5+ captures, coherent thesis detected)
      → ready (clear thesis + evidence + structure)
        → draft (draft generated)
          → published (exported to platform)
```

## Think History (`explorations/*-think-history.json`)

```json
{
  "exploration_id": "ai-agent-routing",
  "sessions": [
    {
      "date": "2026-03-23T15:00:00+08:00",
      "questions_asked": 5,
      "new_captures": ["2026-03-23-th-abc"],
      "suggested_outline": [
        "Hook — Everyone starts with if-else routing",
        "The Shuri incident",
        "Why static ≠ wrong",
        "What context-aware might mean",
        "The evolution path"
      ],
      "thesis_before": "Routing is hard",
      "thesis_after": "Routing is a product design problem"
    }
  ]
}
```

## Draft Format (`drafts/*.md`)

```markdown
---
exploration: ai-agent-routing
version: 1
language: en
word_count: 487
created: 2026-03-23T16:00:00+08:00
angle: "I know my routing is wrong, and I ship it anyway"
format: article | brief | thread
sources: [2026-03-19-abc, 2026-03-20-def, ...]
---

Draft content in markdown.
```

## Style Memory (`memory/style-*.md`)

```markdown
## Voice Profile (English)

- Tone: Casual-professional, conversational
- Sentence rhythm: Short declarative, then longer explanatory
- Vocabulary: Prefers "ship" over "deploy", "break" over "fail"
- Structure: Opens with anecdote, uses numbered lists for technical points
- Avoids: Corporate jargon, excessive hedging, emoji in body text
- Signature patterns: "Here's the thing though:" as pivot phrase
- Paragraph length: 2-4 sentences typically
- Uses code examples: Yes, inline and blocks

Last updated: 2026-03-23
Source drafts: ai-agent-routing, context-engineering
```

## Config (`config.json`)

```json
{
  "version": "0.1.0",
  "default_language": "en",
  "created": "2026-03-23T10:00:00+08:00"
}
```

## YouTube Cache (`youtube/*.json`)

```json
{
  "video_id": "dQw4w9WgXcQ",
  "url": "https://youtube.com/watch?v=dQw4w9WgXcQ",
  "title": "The Future of AI Agents",
  "channel": "Andrej Karpathy",
  "duration": "2:34:17",
  "indexed_at": "2026-03-23T14:00:00+08:00",
  "summary": "...",
  "key_arguments": [...],
  "quotes": [...],
  "selected_insights": ["2026-03-23-yt-abc123"]
}
```

## Design Principles

1. **Human-readable.** Everything is markdown or JSON. No binary formats.
2. **Git-friendly.** Can be version-controlled. Diffs are meaningful.
3. **Forward-compatible.** V2 macOS app reads the same format. No migration needed.
4. **Append-only pool.** Captures are never deleted, only explorations evolve.
5. **Portable.** Copy `~/.lumen/` to another machine and everything works.
