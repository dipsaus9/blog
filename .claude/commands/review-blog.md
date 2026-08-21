---
description: Review a draft post against the house style guide
argument-hint: <path to .md file>
---

Review the blog post at:

$ARGUMENTS

Read `CLAUDE.md` first, then review the file against it. Do **not** rewrite the whole post — report findings and offer targeted edits. Check:

1. **Voice** — first person, conversational, opinionated, analogy-driven. Flag dry, generic, or hype-filled passages and suggest sharper alternatives.
2. **Structure** — intro hooks and sets a roadmap; sections are focused and subheaded; a `## Table of contents` exists and its anchors match the section titles. For a new post, frontmatter is present and `slug` matches the filename.
3. **Code blocks** — each is introduced by a lead-in sentence; examples use `tsx`/`scss`; code follows Prettier rules (no semicolons, single quotes, 2-space indent).
4. **Links & images** — external links resolve to real, relevant sources; image paths point at `images/<slug>/` and the referenced files exist. List any missing image files and any broken/placeholder links.
5. **Formatting** — prose is one line per paragraph (no manual wrapping). Run `npm run format` and report any files that fail the check.

Output: a short prioritized list of issues (must-fix vs. nice-to-have), each with a concrete suggested fix. Ask before applying edits.
