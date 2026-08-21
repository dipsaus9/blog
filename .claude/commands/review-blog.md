---
description: Review a draft post against the house style guide
argument-hint: <path to .md file>
---

Review the blog post at:

$ARGUMENTS

Read `STYLE.md` in full first — it is the voice specification. Read `CLAUDE.md` for file and formatting conventions. Do **not** rewrite the whole post — report findings and offer targeted edits. Check:

1. **Paragraph rhythm** — the target drafts miss most often, so measure it first. Report the full distribution of paragraph lengths in sentences, not just the average: mean (4.5–6), standard deviation (**must be ≥ 1.5**), and the share held by the single most common length (**must be under 35%**). A post can hit the average perfectly and still be flat. Also count standalone one-sentence paragraphs: **more than two in a post is a finding**, and a run of them is the worst version of it. Call it out explicitly when the distribution clumps or when the post stacks statements instead of telling a story, and name the paragraphs to break up or merge.
2. **Density** — for every paragraph, name the one thing it adds. List any paragraph you cannot justify, quote its first sentence, and recommend cutting it. Do the same for padding clauses that only soften, re-announce or label something as interesting.
3. **Voice fingerprint** — mean sentence length (14–17), sentences over 30 words (under 5%), share under 8 words (under 10%), "you" density (18–27 per 1k), "I" density (2–4), analogy hits (4–9), ellipses (zero). When measuring sentence length, first mask URLs and decimals (`8.5%`, `jetbrains.com`) or the splitter breaks on them and reports far more short sentences than the post has. Flag dry, generic or inflated passages with sharper alternatives.
4. **The seven moves** — does it open on the analogy with no throat-clearing? Is there exactly one analogy, present in every section, and dropped explicitly when it stops explaining? Is "you" dominant? Does code build up in order? Is there a stance with an honest carve-out? Does Dennis admit a mistake of his own? Does it close on the analogy plus a short recap?
5. **Anti-AI checklist** — run every item in the `STYLE.md` checklist and quote each offending line with its replacement. Also run the two tests: can two body paragraphs swap without breaking the post, and does every paragraph add something new?
6. **Vocabulary & grammar** — flag any banned word from `STYLE.md`. Fix mechanical grammar errors silently (article agreement, stray capitals, subject–verb) but never upgrade phrasing or swap a plain word for a fancier one.
7. **Structure** — sections are focused and subheaded; headings are sentence case; a `## Table of contents` exists and its anchors match the section titles. Frontmatter is present and `slug` matches the filename.
8. **Code blocks** — each is introduced by a lead-in sentence; examples use `tsx`/`scss`; code follows Prettier rules (no semicolons, single quotes, 2-space indent).
9. **Links & images** — external links resolve to real, relevant sources; image paths point at `images/<slug>/` and the referenced files exist. List any missing image files and any broken/placeholder links.
10. **Formatting** — prose is one line per paragraph (no manual wrapping). Run `npm run format` and report any files that fail the check.

Output: a short prioritized list of issues (must-fix vs. nice-to-have), each with a concrete suggested fix. Ask before applying edits.
