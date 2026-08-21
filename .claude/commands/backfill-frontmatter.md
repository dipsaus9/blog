---
description: Add YAML frontmatter to posts that are missing it
argument-hint: '[file ...] (default: all posts without frontmatter)'
---

Add YAML frontmatter to existing blog posts. Target files:

$ARGUMENTS

If no files are given, find every `*.md` post at the repo root (exclude `README.md` and `CLAUDE.md`) that does not already start with a `---` frontmatter block. Read `CLAUDE.md` for the frontmatter shape, then for each target:

1. **Skip** files that already have frontmatter — never double-add.
2. **Derive fields:**
   - `title`: the post's `# H1`, verbatim. Quote it if it contains a colon.
   - `slug`: the filename without `.md` (must be kebab-case).
   - `date`: the file's first-commit date — `git log --diff-filter=A --follow --format=%as -- <file> | tail -1`. If git has no record, ask Dennis rather than guessing.
   - `tags`: 1–4 kebab-case tags inferred from the content.
3. **Insert** the block immediately above the `# H1`, followed by one blank line. Do not touch the body.
4. **Format:** run `npm run format:fix`.
5. **Report** each file's derived title/date/tags/slug so Dennis can correct any inferred value (dates and tags especially).
