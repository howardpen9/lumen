# /lumen-review — Weekly learning review (Learning Mirror)

## Preamble

Before executing, verify:
1. `~/.lumen/pool/` exists and has captures
2. If no captures in the review period, show encouragement: "Quiet week. That's OK. What have you been consuming — any videos, articles, conversations worth capturing?"
3. Create `~/.lumen/reviews/` if it doesn't exist
4. Check for previous reviews in `~/.lumen/reviews/` to compare week-over-week

## Purpose

Show the user their intellectual growth over the past week. This is the "Learning Mirror" — it reflects what you've been thinking, how your ideas evolved, and what's ready for output.

## Usage

```
/lumen-review                   # This week (default)
/lumen-review --period 30d      # Last 30 days
/lumen-review --all             # All time
```

## Workflow

### Step 1: Gather Data

Scan `~/.lumen/pool/`, `~/.lumen/explorations/`, `~/.lumen/drafts/`, `~/.lumen/exports/` for the review period.

### Step 2: Generate Report

```
✦ Your Week in Review (Mar 17 – 23, 2026)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  23 thoughts captured · 7 voice · 4 new connections · 1 published

  Biggest shift: You went from thinking about routing as a
  technical problem to framing it as a product design challenge.

  Explorations:
  ● AI Agent Routing ████████░░ 80% → Crystallizing (+3 this week)
  ○ Context Engineering ███░░░░░░░ 30% → Exploring (new!)
  ◌ Tauri vs Electron █░░░░░░░░░ 10% → Seed (dormant)

  Connections discovered:
  → Karpathy's routing argument ↔ your Agent Hub experience
  → AttnRes paper ↔ context compression work
  → "Product design" framing is a bridge between both explorations

  You're close to publishing on "AI Agent Routing."
  The gap between your beliefs and your code could be the hook.

  → /lumen-think agent-routing   (deepen)
  → /lumen-draft agent-routing   (start writing)
```

### Step 3: Offer Actions

Based on the review:
- Crystallizing → suggest /lumen-think or /lumen-draft
- Dormant → "Still interested in X? Want to capture more?"
- Published → "How did it perform? Want to capture reactions?"

## Operating Principles

1. **Celebrate growth.** Always highlight what's new, not what's missing.
2. **Track evolution.** Show how positions shifted ("last week you said X, now you say Y").
3. **Proactive suggestions.** Don't just report — recommend next actions.
4. **Honesty.** If the user hasn't captured anything: "Quiet week. That's OK. What have you been reading?"

## Data Dependencies

- Reads: All `~/.lumen/` subdirectories
- Writes: `~/.lumen/reviews/{date}-review.md` (save the review)
