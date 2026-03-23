# Lumen — Thinking & Writing Skills for Claude Code

## What is Lumen?

Lumen is a set of Claude Code skills that turn your CLI into a thinking + writing tool.
Like gstack is for coding, Lumen is for thinking.

**First Principle:** Help people boost productivity by collecting, indexing, and activating their knowledge.

## Skills

| Skill | Purpose | Phase |
|-------|---------|-------|
| `/lumen-capture` | Quick capture a thought, URL, or YouTube video | Core |
| `/lumen-index` | Show your knowledge pool, auto-categorized | Core |
| `/lumen-think` | Q&A session to deepen an exploration | Core |
| `/lumen-draft` | Generate a draft from accumulated thoughts | Core |
| `/lumen-polish` | Polish in your style (zh/en) | Output |
| `/lumen-export` | Format for X thread / LinkedIn / blog | Output |
| `/lumen-review` | Weekly learning review | Reflection |
| `/lumen-youtube` | Index a YouTube video, extract insights | Capture |

## Data Storage

All data lives in `~/.lumen/`:

```
~/.lumen/
├── pool/                    # Raw captures (one .md per thought)
│   ├── 2026-03-23-abc123.md
│   └── ...
├── explorations/            # AI-grouped topic clusters
│   ├── ai-agent-routing.json
│   └── ...
├── drafts/                  # Generated drafts
│   ├── ai-agent-routing-draft-1.md
│   └── ...
├── memory/                  # Style memory + persona
│   ├── style-zh.md
│   ├── style-en.md
│   └── persona.json
├── youtube/                 # YouTube transcript cache
│   └── ...
└── config.json              # User preferences
```

## Core Flow

```
/lumen-capture → /lumen-index → /lumen-think → /lumen-draft → /lumen-polish → /lumen-export
```

Each skill reads from and writes to `~/.lumen/`. Skills can be used independently or in sequence.

## Development

```bash
npm run gen:skill-docs    # Regenerate SKILL.md from .tmpl templates
npm run setup             # Install skills to ~/.claude/skills/lumen/
```

## Design Principles

1. **One AskUserQuestion per decision.** Never batch. Re-ground context each time.
2. **User makes judgment calls, AI does everything else.** Categorization, linking, formatting = AI. "Is this important?" = human.
3. **Completeness over shortcuts.** Marginal cost of thorough analysis ≈ zero with AI.
4. **Skills feed into each other.** /capture output → /index reads it. /think output → /draft reads it.
5. **Local-first.** All data in ~/.lumen/. No cloud. Git-friendly.
6. **Leverage existing subscription.** Zero cost to user beyond their Claude Code plan.
