# Blogs

A collection of blogs I've written over the years, mostly about design systems, atomic design, and frontend architecture. Posts are written here in Markdown and published externally (Medium / Divotion).

## Structure

```
.
├── <post-slug>.md              # one Markdown file per post, at the repo root
├── images/
│   └── <post-slug>/            # images for a post live in a folder named after its slug
│       └── *.jpg|png|svg
├── STYLE.md                    # personal writing style guide (the voice spec)
├── CLAUDE.md                   # conventions for AI-assisted writing
├── .claude/commands/           # authoring slash commands (/new-blog, /review-blog)
└── prettier.config.mjs         # formatting rules
```

## Conventions

- **One file per post** at the repo root, named in `kebab-case.md` (e.g. `what-is-a-design-system.md`).
- **New posts** start with a YAML frontmatter block, then `# Title`, an intro, and a table of contents.
- **Voice is specified in [STYLE.md](STYLE.md)** — analogy-led openings, one sustained metaphor per post, second-person teaching, incremental code, stances with carve-outs. It also carries an anti-AI checklist to run before publishing.
- **Images** go in `images/<post-slug>/` and are referenced with relative paths and descriptive alt text: `![alt](images/<slug>/name.png)`.

## Writing a new post

The fastest path is the slash commands (run inside Claude Code):

- `/new-blog <topic or angle>` — scaffolds a new post: creates the file with frontmatter and a section skeleton, the image folder, and drafts in the blog voice.
- `/review-blog <file>` — reviews a draft against the style guide (voice, structure, links, image paths) and formatting.
- `/backfill-frontmatter [file ...]` — adds YAML frontmatter to older posts that are missing it (dates come from git history).

Prefer these over writing from scratch — they encode the house style so posts stay consistent.

## Formatting

Prose is left unwrapped (one line per paragraph); code blocks follow the Prettier config (no semicolons, single quotes, 2-space indent).

```sh
npm install          # first time only
npm run format       # check formatting
npm run format:fix   # apply formatting
```

## Publishing

Posts are drafted here and published to external platforms (Medium, Divotion). This repo is the source of truth and archive. When publishing, upload the images and fix up relative paths for the target platform.
