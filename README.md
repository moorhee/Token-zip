# Token Zip

A Claude Code skill that compresses every response to high-signal, low-token output and routes each task to the right Claude model or depth tier based on complexity, stakes, and output type.

Always active — governs *how tightly* every response is produced, regardless of topic or which other skills are running. It is a style and routing layer, not a content producer: other skills own *what* gets produced (a SQL query, a rewrite, a QA finding); Token Zip owns how much filler surrounds it.

## What's here

- `skills/token-zip/SKILL.md` — the skill itself (frontmatter + instructions). This is the source of truth.

## Installing

Package `skills/token-zip/` as a `.skill` file (a zip archive of that folder) and install it in Claude Code, or copy the `skills/token-zip/` folder into your skills directory.

To rebuild the package from this repo:

```bash
cd skills && zip -X -r ../token-zip.skill token-zip -x "*.DS_Store"
```

(run from the repo root)

## Maintenance

Model IDs in `SKILL.md` are pinned and go stale. Verify them against Anthropic's documentation periodically; the maintenance note in the file records when they were last checked.

## Core principle

**Shortest complete answer wins.** One line if one line is correct and complete. Five bullets if the task needs five. Never pad to look thorough, never truncate to look efficient.

## How it decides

**Precedence when rules collide:**
1. Explicit user instruction ("walk me through it") overrides everything
2. An active content-producing skill owns the output format
3. Token Zip governs tightness inside that format

**Always cut:** restating the request, preambles, closing offers, filler transitions, unrequested background, narrating what you're about to do.

**Always keep:** the direct answer first, usable code/SQL/output, caveats that would change a decision, explicit flags for missing info, tradeoffs when more than one option is reasonable.

## Depth routing (conversation)

| Tier | When |
|---|---|
| **Fast** | Status checks, single-fact questions, yes/no confirmations |
| **Balanced** (default) | SQL, QA review, drafts, data pulls, multi-step tasks |
| **Deep** | Multi-factor analysis, high-stakes decisions, teaching, emotionally loaded context |

Overrides: "quick"/"brief" → Fast. "Walk me through"/"explain" → Deep. Emotional context → never Fast.

## Model routing (API / automated contexts)

| Tier | Model | Use for |
|---|---|---|
| Fast | Haiku | Lookups, extraction, classification, yes/no checks |
| Balanced | Sonnet | SQL, data pulls, QA, drafts, multi-step workflows |
| Deep | Opus | Competing interpretations, high-stakes recommendations |

Defaults to Sonnet. Escalates to Opus only when reasoning is genuinely hard *and* a wrong answer costs something. Drops to Haiku for anything a one-line lookup handles.

## Output shape by task type

SQL/data → query first in a code block, minimal context. QA/review → findings first, severity-ordered, one bullet per issue. Drafts/rewrites → deliver the rewrite, no commentary on the original. Strategy → 1–3 options with tradeoffs, then a recommendation.

## Formatting rules

Short paragraphs, bullets over prose, code blocks for anything copyable, no headers under ~150 words, no em/en dashes in user-facing deliverables (reads as AI-generated).
