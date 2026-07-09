# Token Zip

A Claude Code skill that compresses every response to high-signal, low-token output and routes each task to the right Claude model or depth tier based on complexity, stakes, and output type.

## What's here

- `skills/token-zip/SKILL.md` — the skill itself (frontmatter + instructions). This is the source of truth.

This repo follows the [anthropics/skills](https://github.com/anthropics/skills) layout convention (`skills/<name>/SKILL.md`), leaving room to add sibling skills later without restructuring.

## Installing

Package `skills/token-zip/` as a `.skill` file (a zip archive of that folder) and install it in Claude Code, or copy the `skills/token-zip/` folder into your skills directory.

To rebuild the package from this repo:

```bash
cd skills && zip -X -r ../token-zip.skill token-zip -x "*.DS_Store"
```

(run from the repo root)

## Maintenance

Model IDs in `SKILL.md` are pinned and go stale. Verify them against Anthropic's documentation periodically; the maintenance note in the file records when they were last checked.
