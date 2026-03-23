# /lumen-youtube — Index a YouTube video into your knowledge pool

## Preamble

Before executing, verify:
1. A YouTube URL is provided. If not, ask for one.
2. Create `~/.lumen/youtube/` if it doesn't exist
3. Check if this video was already indexed (look in `~/.lumen/youtube/{video-id}.json`)
4. If already indexed, show previous summary and ask: "Want to re-index or review previous insights?"
5. Load existing explorations for connection matching

**Hard gate:** Use WebFetch to get the actual page content. Never fabricate video content or quotes. If transcript is unavailable, say so clearly and offer alternatives.

## Purpose

Turn a YouTube video into structured knowledge. AI fetches the transcript, summarizes key arguments, extracts quotes, and connects insights to your existing explorations.

No one has time to finish every video. Lumen watches for you and lets you judge what matters.

## Usage

```
/lumen-youtube https://youtube.com/watch?v=...
/lumen-youtube https://youtu.be/...
```

## Workflow

### Step 1: Fetch Video Info

Use WebFetch on the YouTube URL to extract:
- Video title, channel name, duration
- Description text
- Transcript (if available via page content)

If transcript is not directly available, inform the user and offer alternatives:
- "Paste the transcript manually"
- "I'll work with the description and your notes about it"

### Step 2: AI Summary

Generate structured summary:

```
✦ Indexed: "The Future of AI Agents" — Andrej Karpathy
  Duration: 2h 34min · Channel: Karpathy · Views: 2.4M

  Key Arguments:
  1. Single-model agents hit a ceiling; multi-agent routing is the future
  2. Context management is the hardest unsolved problem
  3. Tool use will evolve into persistent agents with memory

  Notable Quotes:
  • "The routing decision is a product design problem" (~1:24:30)
  • "Context windows are the new memory" (~1:52:15)
  • "We're building systems that think, not tools that respond" (~2:10:00)

  Connected to your explorations:
  → AI Agent Routing (high relevance — directly supports your thesis)
  → Context Window as Memory (medium — the selective attention point)

  New topic detected:
  → "Persistent agents with memory" — not in your pool yet
```

### Step 3: User Judgment

This is the key step. Ask the user to mark what matters:

> Which insights are worth keeping? (Select all that apply)
> 1. ✓ "Routing is a product design problem" → add to AI Agent Routing
> 2. ✓ "Context windows are the new memory" → add to Context Engineering
> 3. ○ "Systems that think, not tools that respond" → new exploration?
> 4. ○ Skip — not relevant to my current thinking

### Step 4: Save

For each marked insight, create a capture in `~/.lumen/pool/`:
```markdown
---
id: 2026-03-23-yt-abc123
type: youtube
source: youtube
url: https://youtube.com/watch?v=...
video_title: "The Future of AI Agents"
channel: Andrej Karpathy
timestamp: ~1:24:30
tags: [agent-routing, product-design]
related_explorations: [ai-agent-routing]
---

"The routing decision is fundamentally a product design problem, not an engineering one."

From Karpathy's talk on AI agents. Supports my thesis that routing should be context-aware.
Connects to the Shuri incident — exactly the kind of cross-category failure he describes.
```

### Step 5: Update Explorations

Add the new captures to relevant explorations. Update status if a threshold is crossed.

## Operating Principles

1. **User judges, AI summarizes.** Don't auto-save everything. Present and let the user choose.
2. **Timestamps matter.** Help users find the exact moment in the video later.
3. **Connections are gold.** The most valuable output is linking video insights to existing explorations.
4. **One video = multiple explorations.** A 2h talk might touch 5 different topics in the user's pool.
5. **No transcript? Still useful.** Even the description + user's notes create value.

## Data Dependencies

- Reads: `~/.lumen/explorations/*.json` (for connections)
- Writes: `~/.lumen/pool/{date}-yt-{hash}.md` (selected insights)
- Writes: `~/.lumen/youtube/{video-id}.json` (full summary cache)
- Updates: `~/.lumen/explorations/*.json` (add connections)

## Downstream

- Think about connected exploration → `/lumen-think`
- See updated knowledge pool → `/lumen-index`
