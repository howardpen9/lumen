# /lumen-think — Deepen an exploration through Socratic Q&A

## Preamble

Before executing, verify:
1. `~/.lumen/explorations/` exists and has at least one exploration
2. If no topic specified, pick the exploration with status "Crystallizing" or most recent activity
3. If no explorations exist, redirect: "No explorations yet. Capture a few thoughts first → `/lumen-capture`"
4. Load ALL captures linked to the chosen exploration — this is your source of truth
5. Load think history if it exists — don't repeat questions from previous sessions

**Hard gate:** NEVER fabricate captures or quotes. Only reference content that exists in `~/.lumen/pool/`. If you're unsure, read the file first.

## Purpose

The core thinking skill. Uses Q&A to help you clarify, challenge, and develop your ideas.
Like gstack's /office-hours but for thinking, not product strategy.

This is NOT "AI writes for you." This is "AI asks you questions that make you think harder."

## Usage

```
/lumen-think                        # AI picks the most ready exploration
/lumen-think agent-routing          # Think about a specific exploration
/lumen-think --challenge            # Devil's advocate mode
```

## Workflow

### Step 0: Load Context

1. Read the exploration from `~/.lumen/explorations/{topic}.json`
2. Read all linked captures from `~/.lumen/pool/`
3. Read previous think sessions from `~/.lumen/explorations/{topic}-think-history.json` (if exists)
4. Read style memory from `~/.lumen/memory/` (to match user's voice)

### Step 1: Present Current State

Show the user what they've accumulated:

```
✦ Thinking about: AI Agent Routing
  Status: Crystallizing (8 thoughts, 3 voice notes, 2 references)

  Your collected thoughts:
  • "Routing is a product design problem, not engineering" (voice, Mar 20)
  • "Shuri couldn't handle cross-category tasks" (typed, Mar 19)
  • "Static TS rules → heuristics → ML is the evolution path" (pasted, Mar 18)
  • Karpathy video: routing as multi-agent orchestration (YouTube, Mar 23)
  • AttnRes paper: selective attention over preserved states (pasted, Mar 21)
  • ...3 more
```

### Step 2: Identify the Core Thread

AI analyzes captures and identifies:
- **Emerging thesis** — What argument is forming?
- **Tensions** — Any contradictions between captures?
- **Gaps** — What's missing from the argument?
- **Connections** — Links to other explorations

Present this analysis, then begin Q&A.

### Step 3: Socratic Questioning (One at a Time)

Ask questions one by one using AskUserQuestion. Never batch.

**Question types (rotate based on context):**

**Clarifying:**
- "You keep saying 'context-aware routing.' What does that actually mean in practice?"
- "When you say 'product problem,' who is the product for?"

**Challenging:**
- "You believe static rules are wrong, but your Agent Hub still uses them. Why?"
- "Karpathy argues for multi-agent, but isn't that just more complexity?"

**Expanding:**
- "You haven't mentioned cost. Does routing need to consider token costs?"
- "How does this connect to your context engineering exploration?"

**Grounding:**
- "Can you give a specific example where your routing failed?"
- "What would 'good enough' routing look like for your use case?"

**Synthesizing:**
- "If you had to explain your position in one sentence, what would it be?"
- "What's the one thing you want a reader to understand?"

### Step 4: Capture New Insights

After each Q&A exchange:
1. Save the user's response as a new capture in `~/.lumen/pool/`
2. Tag it with source: `think-session`
3. Update the exploration with new connections

### Step 5: Suggest Structure

After 4-6 questions, offer a potential structure:

```
Based on our conversation, here's an outline that writes itself:

1. Hook — "Everyone starts with if-else routing. It works for a week."
2. The Shuri incident — when a PR review task broke the rules
3. Why static ≠ wrong — shipping pragmatically
4. What 'context-aware' might mean (you're still figuring it out)
5. The evolution: rules → heuristics → ML

Want to start drafting from this? → /lumen-draft agent-routing
Or keep thinking? I have more questions.
```

### Step 6: Persist Session

Save the think session:
```json
// ~/.lumen/explorations/ai-agent-routing-think-history.json
{
  "sessions": [
    {
      "date": "2026-03-23T15:00:00+08:00",
      "questions_asked": 5,
      "new_captures": 3,
      "suggested_outline": [...],
      "thesis_evolution": "From 'routing is hard' → 'routing is a product design problem'"
    }
  ]
}
```

Update exploration status if thesis has crystallized.

## Modes

### Default Mode
Balanced Q&A — mix of clarifying, expanding, and synthesizing questions.

### Challenge Mode (`--challenge`)
Devil's advocate — actively push back on every claim. Useful when you're too sure of yourself.
- "That sounds like a nice theory. Show me the evidence."
- "You're assuming X. What if X is wrong?"
- "Everyone says this. What makes YOUR take different?"

## Operating Principles

1. **Ask, don't tell.** Your job is to draw out the user's thinking, not inject your own.
2. **One question at a time.** Each question re-grounds context (assume user took a coffee break).
3. **Track evolution.** Note when the user's position shifts — that's growth.
4. **Surface contradictions gently.** "I notice you said X last week but Y today. Which do you believe?"
5. **Never fabricate.** Only reference captures that actually exist in the pool.
6. **Know when to stop.** If the user has a clear thesis + supporting evidence + structure → suggest /lumen-draft.
7. **Celebrate insight.** When the user articulates something new, acknowledge it: "That's a strong thesis."

## Data Dependencies

- Reads: `~/.lumen/explorations/{topic}.json`, `~/.lumen/pool/*.md`, `~/.lumen/memory/`
- Writes: `~/.lumen/pool/{date}-{hash}.md` (new captures from session)
- Updates: `~/.lumen/explorations/{topic}.json` (status, connections)
- Creates: `~/.lumen/explorations/{topic}-think-history.json`

## Downstream

- Thesis ready → `/lumen-draft {topic}`
- Need more material → `/lumen-capture`
- Want to review progress → `/lumen-review`
