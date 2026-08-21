# CLAUDE.md

Guidance for Claude when helping write and edit blog posts in this repo.

## What this repo is

A personal collection of technical blog posts by Dennis (author: "Dipsaus"). Each post is a standalone Markdown file at the repo root. Posts are drafted here and published externally (Medium / Divotion). Recurring topics: design systems, design tokens, atomic design, compound components, React/TypeScript, frontend architecture.

## File & folder conventions

- One post per file at the repo root, named in `kebab-case.md`.
- The slug is the filename without `.md` (e.g. `what-is-a-design-system`).
- Images for a post live in `images/<slug>/`. Reference them with relative paths and descriptive alt text: `![Naming conventions can be hard](images/<slug>/naming-conventions.jpeg)`.
- Do not invent image files. If a post needs an image that doesn't exist yet, insert the Markdown reference to the intended path and leave a `<!-- TODO: image -->` note so Dennis can add the asset.

## Post structure

New posts use YAML frontmatter, then the body:

```markdown
---
title: Human Readable Title
date: YYYY-MM-DD
tags: [design-systems, react]
slug: kebab-case-slug
---

# Human Readable Title

Intro paragraph: hook the reader, state what the article covers and what they'll walk away with. First person, conversational.

## Table of contents

- [Section One](#section-one)
- [Section Two](#section-two)

## Section One

...
```

- Frontmatter `slug` must match the filename.
- `date` is the original publish date (the file's first-commit date for existing posts; today's date for new ones unless told otherwise).
- All posts now carry frontmatter. Use `/backfill-frontmatter` to add it to any post that's still missing it.
- Anchor links in the table of contents are the lowercased section title with spaces replaced by hyphens and punctuation removed.

## Voice & style

Match the existing posts. Key traits:

- **First person, conversational.** "In my opinion…", "I'll dive into…", "Let's take a closer look." Address the reader directly as "you".
- **Lead with a hook, then a roadmap.** The intro tells the reader what's coming and why it matters.
- **Analogies carry the explanation.** The posts lean on real-world analogies (a city's roads and neighborhoods; the periodic table; subatomic particles). Use a concrete analogy to introduce an abstract concept, then connect it back.
- **Opinionated but generous.** Take a clear stance ("a design system is a way of working"), but link out to and credit other sources fairly.
- **Bold key terms** on first meaningful use. Use numbered/bulleted lists for benefits and enumerations.
- **Blockquotes for definitions** — the money-quote definition of a concept goes in a `>` blockquote.
- **Code with a lead-in.** Every code block is introduced by a sentence saying what it shows. Code examples are TypeScript/React (`tsx`) or `scss`.
- **Link out inline** with Markdown links to sources, tools, and prior articles.

Avoid: filler and hype, unexplained jargon, walls of text without a subheading, code blocks dropped in without explanation.

## Formatting

Prettier config (`prettier.config.mjs`): `printWidth: 50000`, `proseWrap: always`, `semi: false`, `singleQuote: true`, `tabWidth: 2`, `trailingComma: all`.

- **Prose is one line per paragraph** — do not hard-wrap prose. The huge `printWidth` plus `proseWrap: always` collapses each paragraph to a single line. Never insert manual line breaks inside a paragraph.
- **Code blocks** follow the Prettier rules above: no semicolons, single quotes, 2-space indent, trailing commas.
- After writing or editing, run `npm run format:fix` to normalize.

## Workflow when asked to write or edit

1. Confirm the topic/angle and target slug.
2. For a new post, prefer scaffolding via `/new-blog` (`.claude/commands/`).
3. Draft in the voice above; keep sections focused and subheaded.
4. Keep image references pointing at `images/<slug>/`; flag missing assets.
5. Run `npm run format:fix` and report anything that needs a real image or a fact Dennis should verify.
