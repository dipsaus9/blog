---
description: Review a draft post against the house style guide
argument-hint: <path to .md file>
---

Review the blog post at:

$ARGUMENTS

Read `STYLE.md` in full first — it is the voice specification. Read `CLAUDE.md` for file and formatting conventions. Do **not** rewrite the whole post — report findings and offer targeted edits. Check:

1. **Voice fingerprint** — measure and report against the `STYLE.md` targets: mean sentence length (14–17), share of sentences under 8 words (~18%), "you" density (18–27 per 1k words), "I" density (2–4), analogy hits (4–9), ellipses (must be zero). Flag dry, generic or inflated passages with sharper alternatives.
2. **The seven moves** — does it open on the analogy with no throat-clearing? Is there exactly one analogy, present in every section, and dropped explicitly when it stops explaining? Is "you" dominant? Does code build up in order? Is there a stance with an honest carve-out? Does Dennis admit a mistake of his own? Does it close on the analogy plus a short recap?
3. **Anti-AI checklist** — run every item in the `STYLE.md` checklist and quote each offending line with its replacement. Also run the two tests: can two body paragraphs swap without breaking the post, and does every paragraph add something new?
4. **Vocabulary & grammar** — flag any banned word from `STYLE.md`. Fix mechanical grammar errors silently (article agreement, stray capitals, subject–verb) but never upgrade phrasing or swap a plain word for a fancier one.
5. **Structure** — sections are focused and subheaded; headings are sentence case; a `## Table of contents` exists and its anchors match the section titles. Frontmatter is present and `slug` matches the filename.
6. **Code blocks** — each is introduced by a lead-in sentence; examples use `tsx`/`scss`; code follows Prettier rules (no semicolons, single quotes, 2-space indent).
7. **Links & images** — external links resolve to real, relevant sources; image paths point at `images/<slug>/` and the referenced files exist. List any missing image files and any broken/placeholder links.
8. **Formatting** — prose is one line per paragraph (no manual wrapping). Run `npm run format` and report any files that fail the check.

Output: a short prioritized list of issues (must-fix vs. nice-to-have), each with a concrete suggested fix. Ask before applying edits.
