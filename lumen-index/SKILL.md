# /lumen-index — View and manage your knowledge pool

## Preamble

Before executing, verify:
1. `~/.lumen/pool/` exists and contains captures. If empty, redirect: "No captures yet. Try `/lumen-capture` first."
2. Read all `~/.lumen/pool/*.md` files — parse YAML frontmatter for metadata
3. Read all `~/.lumen/explorations/*.json` files for existing clusters
4. Count: total captures, total explorations, last capture timestamp

## Purpose

Show the user their accumulated knowledge, auto-organized by AI into explorations.
This is the "Learning Mirror" — it reflects what you've been thinking about.

## Usage

```
/lumen-index                    # Show all explorations, sorted by activity
/lumen-index --recent           # Show captures from last 7 days
/lumen-index --topic routing    # Filter by topic keyword
```

## Workflow

### Step 1: Scan Knowledge Pool

Read all files from:
- `~/.lumen/pool/*.md` — Raw captures (parse frontmatter for metadata)
- `~/.lumen/explorations/*.json` — Existing exploration clusters

### Step 2: Auto-Cluster (if needed)

If there are captures not yet assigned to explorations:
1. Extract key concepts from unassigned captures
2. Compare against existing explorations (cosine similarity of tags/keywords)
3. Group related captures into explorations
4. Create new explorations for clusters of 3+ related captures

Exploration JSON format:
```json
{
  "id": "ai-agent-routing",
  "title": "AI Agent Routing",
  "status": "crystallizing",
  "captures": ["2026-03-19-abc", "2026-03-20-def", ...],
  "tags": ["routing", "multi-agent", "product-design"],
  "created": "2026-03-19T...",
  "updated": "2026-03-23T...",
  "ai_summary": "Your evolving position: routing is a product design problem...",
  "connections": ["context-engineering"]
}
```

### Step 3: Display Dashboard

Present as a clear overview:

```
✦ Your Knowledge Pool
  12 thoughts · 3 explorations · last capture: 2 hours ago

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  ● AI Agent Routing                    Crystallizing
    8 thoughts · 3 voice · 2 refs · evolving since Mar 19
    "Routing is a product design problem, not engineering."
    → Ready to think deeper? Try /lumen-think agent-routing

  ○ Context Window as Memory            Exploring
    3 thoughts · 1 paper · new since Mar 21
    "Selective attention over preserved states."

  ◌ Tauri vs Electron                   Seed
    2 thoughts · pasted from notes · 1 week ago
    Decision pending — waiting for more input.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Recent uncategorized:
    "LLM costs are dropping 10x per year" (today)
    → Doesn't match existing explorations. Capture more to form a cluster.
```

### Step 4: Offer Actions

After displaying, ask:

> What would you like to do?
> 1. Deep-think an exploration → /lumen-think {topic}
> 2. Capture something new → /lumen-capture
> 3. Start drafting → /lumen-draft {topic}
> 4. See weekly review → /lumen-review

## Exploration Statuses

| Status | Meaning | Criteria |
|--------|---------|----------|
| **Seed** | Just started | 1-2 captures |
| **Exploring** | Actively collecting | 3-5 captures |
| **Crystallizing** | Position forming | 5+ captures, AI detects coherent thesis |
| **Ready** | Enough to write | Clear thesis + supporting evidence |
| **Published** | Output created | Draft generated or exported |

AI determines status by analyzing:
- Number of captures
- Coherence of ideas (do they form an argument?)
- Presence of evidence/examples
- Whether the user has articulated a position

## Operating Principles

1. **Mirror, don't organize.** Show what the user has been thinking. Don't impose structure.
2. **Surface connections.** Cross-exploration links are the highest-value insight.
3. **Proactive nudges.** "You haven't added to X in 2 weeks — still interested?"
4. **Growth over completeness.** Show what's new and evolving, not a static list.
5. **No empty states.** If pool is empty, guide to first capture.

## Data Dependencies

- Reads: `~/.lumen/pool/*.md`, `~/.lumen/explorations/*.json`
- Writes: `~/.lumen/explorations/*.json` (updates clusters)
- Never deletes: captures are permanent

## Completion Criteria

Index is complete when:
- [x] All captures scanned and counted
- [x] Unassigned captures clustered into explorations (if 3+ related)
- [x] Dashboard displayed with status for each exploration
- [x] At least one actionable suggestion offered
- [x] Exploration JSON files updated on disk

## Downstream

Suggests based on context:
- Crystallizing exploration → `/lumen-think`
- Ready exploration → `/lumen-draft`
- Low activity → `/lumen-capture`
