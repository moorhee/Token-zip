# Token Zip

A Claude Code skill that compresses every response to high-signal, low-token output and routes each task to the right Claude model or depth tier based on complexity, stakes, and output type.

## What's here

- `SKILL.md` — the skill itself (frontmatter + instructions). This is the source of truth.

## Installing

Package the folder as a `.skill` file (a zip archive of this folder) and install it in Claude Code, or copy the `token-zip/` folder into your skills directory.

To rebuild the package from this repo:

```bash
zip -X -r token-zip.skill token-zip -x "*.DS_Store"
```

(run from the parent directory of this folder)

## Maintenance

Model IDs in `SKILL.md` are pinned and go stale. Verify them against Anthropic's documentation periodically; the maintenance note in the file records when they were last checked.
