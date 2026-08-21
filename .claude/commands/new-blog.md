---
description: Scaffold and draft a new blog post in the house style
argument-hint: <topic or angle>
---

You are helping Dennis start a new blog post for this repo. The topic/angle is:

$ARGUMENTS

Read `CLAUDE.md` first for the full voice and structure guide, then:

1. **Settle the essentials** (ask only if genuinely ambiguous — otherwise pick sensible defaults and state them):
   - A working `title` (Human Readable).
   - A `slug` in kebab-case. The new file is `<slug>.md` at the repo root.
   - 1–4 `tags`.
   - `date`: use today's date.
   - Refuse gracefully if a file with that slug already exists — suggest an alternative slug.

2. **Create the file** `<slug>.md` with YAML frontmatter (title, date, tags, slug), the `# Title`, an intro paragraph, a `## Table of contents`, and a skeleton of 4–7 `##` sections with brief placeholder content that matches the argument's angle. Keep the table of contents anchors in sync with the section titles.

3. **Create the image folder** `images/<slug>/` (add a `.gitkeep` if it would otherwise be empty). Wherever the post would benefit from a visual, insert a Markdown image reference to `images/<slug>/<name>.png` and leave a `<!-- TODO: image -->` comment. Do not fabricate image files.

4. **Draft in the blog voice**: first person, conversational, analogy-driven, opinionated. Lead each code block with an explanatory sentence. Prose is one line per paragraph (no manual wrapping).

5. **Format**: run `npm run format:fix`.

6. **Report** what you created, which sections are still stubs, and which images Dennis needs to supply.
