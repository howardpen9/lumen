# /lumen-export — Format draft for a specific platform

## Preamble

Before executing, verify:
1. A draft or polished version exists in `~/.lumen/drafts/`
2. Prefer polished version over raw draft
3. Create `~/.lumen/exports/` if it doesn't exist

## Purpose

Take a polished draft and reformat it for a target platform: X thread, LinkedIn post, blog article, or plain markdown.

## Usage

```
/lumen-export agent-routing --format x-thread
/lumen-export agent-routing --format linkedin
/lumen-export agent-routing --format blog
/lumen-export agent-routing --format all       # Generate all formats
```

## Supported Formats

| Format | Constraints | Style |
|--------|-------------|-------|
| `x-thread` | 280 chars/tweet, 5-8 tweets | Punchy, hook-first, 🧵 numbering |
| `linkedin` | ~1300 chars optimal | Professional, insight-led, line breaks |
| `blog` | No limit | Full article with headers, code blocks |
| `markdown` | No limit | Clean markdown for any platform |

## Workflow

1. Read latest polished draft (or draft if no polish exists)
2. Ask user which format(s) to generate
3. Generate platform-specific version respecting constraints
4. Present for review
5. Save to `~/.lumen/exports/{topic}-{format}.md`
6. Copy to clipboard for easy paste

## Operating Principles

1. **Platform-native feel.** X threads need hooks. LinkedIn needs line breaks. Don't just truncate.
2. **Preserve voice.** The user's style, not generic AI style.
3. **Character counts matter.** Show remaining chars for constrained formats.
4. **One format at a time.** Don't overwhelm with all formats at once unless asked.

## Data Dependencies

- Reads: `~/.lumen/drafts/*.md`
- Writes: `~/.lumen/exports/{topic}-{format}.md`
