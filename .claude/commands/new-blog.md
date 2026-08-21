---
description: Scaffold and draft a new blog post in the house style
argument-hint: <topic or angle>
---

You are helping Dennis start a new blog post for this repo. The topic/angle is:

$ARGUMENTS

Read `STYLE.md` in full first — it is the voice specification and is not optional. Read `CLAUDE.md` for the file and formatting conventions. Then:

1. **Settle the essentials** (ask only if genuinely ambiguous — otherwise pick sensible defaults and state them):
   - A working `title` (Human Readable).
   - A `slug` in kebab-case. The new file is `<slug>.md` at the repo root.
   - 1–4 `tags`.
   - `date`: use today's date.
   - Refuse gracefully if a file with that slug already exists — suggest an alternative slug.

2. **Create the file** `<slug>.md` with YAML frontmatter (title, date, tags, slug), the `# Title`, an intro paragraph, a `## Table of contents`, and a skeleton of 4–7 `##` sections with brief placeholder content that matches the argument's angle. Keep the table of contents anchors in sync with the section titles.

3. **Create the image folder** `images/<slug>/` (add a `.gitkeep` if it would otherwise be empty). Wherever the post would benefit from a visual, insert a Markdown image reference to `images/<slug>/<name>.png` and leave a `<!-- TODO: image -->` comment. Do not fabricate image files.

4. **Pick the analogy before writing a single section.** One real-world analogy carries the whole post. Propose it to Dennis with an analogy → section mapping before drafting the body, and say plainly where you expect it to break down.

5. **Draft against `STYLE.md`**: open on the analogy (no roadmap sentence, no "In this article"), teach in second person, build code up piece by piece with a lead-in per block, take at least one stance with an honest carve-out, and close by returning to the analogy followed by a short recap. Target 1,500–2,500 words, ~6 sections, 2–4 code blocks, at most 2 lists. Prose is one line per paragraph (no manual wrapping).

6. **Self-check against the anti-AI checklist in `STYLE.md`** and fix what it catches before reporting.

7. **Format**: run `npm run format:fix`.

8. **Report** what you created, which sections are still stubs, and which images Dennis needs to supply.
