# Writing style — Dennis (Dipsaus)

This is my personal writing style reference. Every new post follows it. It was built by measuring my own published posts, not from a generic template — the numbers below are what I actually do, and the rules are the choices I made deliberately.

Read this before drafting or reviewing anything in this repo.

## The one-line version

I explain an abstract frontend idea by running a single real-world analogy through the whole post, talking directly to the reader as "you", building the code up piece by piece, and taking a clear stance that admits where it breaks.

## Voice fingerprint

Measured across my four published posts (9,447 words). These are targets, not trivia — a draft that misses them badly is not in my voice.

| Marker                  | Target                  | Why                                                                                       |
| ----------------------- | ----------------------- | ----------------------------------------------------------------------------------------- |
| Mean sentence length    | 14–17 words             | My 2023 posts sat at 12.5–15.3. The 2024 post drifted to 19.9 and reads flatter.          |
| Sentences under 8 words | ~18% of all sentences   | The punch lines. AI writing almost never does this.                                       |
| Sentences over 30 words | under 5%                | If it needs 30 words, it's two sentences.                                                 |
| "you" / "your"          | 18–27 per 1k words      | The dominant pronoun. I'm teaching, not narrating.                                        |
| "I"                     | 2–4 per 1k words        | Present, but not the subject. It shows up for opinions and mistakes.                      |
| Analogy hits            | 4–9 per 1k words        | The signature move. `from-atoms-to-excellence` hit 9.2 and it's my most distinctive post. |
| Contractions            | 5–8 per 1k words        | Conversational, not chatty.                                                               |
| Ellipses (`...`)        | zero                    | I've never used one. Don't start.                                                         |
| Hedges vs assertions    | assertions ~2.5x hedges | Opinionated.                                                                              |

**Length:** 1,500–2,500 words. About 6 sections. 2–4 code blocks. 1–2 lists in the entire post. At least one diagram or screenshot.

Prose-led means prose-led. If a section has become four bullets, it should have been a paragraph.

## Structure: the seven moves

### 1. Open on the analogy, not on context

Lead with the real-world thing. Let the reader sit in it for two or three sentences before naming the technical subject. No "In this article we will explore" — that's a chatbot artifact and it's the one habit from my old posts I'm dropping.

> Every street in a city has a name. Not because the street needs one, but because people need to tell each other where to go.
>
> Design tokens work the same way. The value doesn't need a name. Your team does.

No throat-clearing openers: not "Design systems have become more popular in recent years", not "In today's frontend landscape".

### 2. Pick one analogy and make it load-bearing

One metaphor per post. It runs through every section and it has to actually explain something in each one — a city, a LEGO box, a kitchen, a power strip. Map it explicitly:

```
§ Intro      — a city needs a plan
§ Tokens     — street names
§ Components — buildings on those streets
§ Patterns   — zoning rules
§ Closing    — you're the city planner
```

Never run two competing analogies in one post. Never stretch one past the point where it explains — when it stops working, say so out loud and switch to plain technical language. Naming the breaking point is honest and it's something AI writing never does.

### 3. Teach in second person

"You" is the default. The reader is the one doing the work.

- Yes: "You'll notice your token list grows fastest at the component layer."
- No: "One might observe that token lists tend to grow at the component layer."

"I" appears for three things only: an opinion, a decision I made on a real project, or a mistake I made. Not for narration.

### 4. Build code up piece by piece

Each block adds one part. No deliberate wrong turn, no strawman refactor. Assemble the real thing in order, then show it working together at the end.

```
1. Tabs         (state + context)
2. TabsList     (container)
3. TabTrigger   (switching)
4. TabsContent  (render)
5. Put together
```

Every block gets a short lead-in sentence saying what it shows. My lead-ins are plain and functional — "We start with an Input field", "Design properties like color can be stored as regular variables" — not "Let's dive into the implementation".

Code follows the repo Prettier config: no semicolons, single quotes, 2-space indent, trailing commas. TypeScript/React (`tsx`) or `scss`.

### 5. Take the stance, then name the limit

State the opinion flat. Defend it after. Then say exactly where I'd do the opposite — the carve-out is what makes the stance credible.

> Don't build a component library before you have tokens. It's the wrong order and it will cost you a rewrite.
>
> The exception is a one-product team shipping in a month. There, tokens are overhead you can't afford yet. Ship the components, extract tokens after.

A post with no carve-out anywhere is either too safe or dishonest.

### 6. Admit the mess

Warm and self-deprecating. My own bad decisions are the best examples I have, and using them buys more trust than any amount of authority.

> I wrote a token called `$colors-background-button-primary-hover-dark-mobile`.
>
> I am not proud of it. It shipped. It's probably still there.

Understated. Never a joke that announces itself.

### 7. Close by cashing in the analogy, then a short recap

Return to the opening metaphor and land it. Then a brief conclusion of what the post covered — short, no bullet-by-bullet replay of every section.

> You don't get to finish a city. Streets get renamed, neighborhoods get rebuilt, and the plan changes underneath you.
>
> Your design system is the same. Name things so the next person can find their way around without calling you.

Never end on "The future looks bright", "Only time will tell", or "As we move forward".

## Sentence rhythm

Build a longer explanatory sentence, then cut it off with a very short one. The contrast is the effect.

> A design token is a decision with a name, and the whole point is that the value underneath it can move without the name moving with it. That's the contract.
>
> Break it and your team stops trusting the system.

Vary paragraph length too. A one-sentence paragraph is allowed and should appear a few times per post. Uniform paragraph size is an AI tell.

## Vocabulary

Plain words by default. A more advanced or precise word is fine when it genuinely does work the plain one can't — but sparingly, and never to sound smarter.

**Default to:** use, build, break, fix, name, ship, grow, show, run, keep

**Never:** leverage, utilize, delve, robust, seamless, comprehensive, facilitate, paradigm, realm, landscape, tapestry, embark, cutting-edge, holistic, actionable, best practices, game-changer, ever-evolving, unlock, harness, elevate, streamline, empower

The test: if the plain word says the same thing, the plain word wins.

> "This makes it easier to change" — not "This facilitates a more robust and scalable approach to iterative modification"

**Grammar:** mechanical errors get fixed silently (article agreement, stray capitals, subject-verb). My phrasing, word choice and sentence shapes are never "upgraded" to sound more native. Fix errors, never style.

## Headings

Sentence case. No locked pattern — whatever fits the section. A rigid formula across every heading reads sing-song over a long post.

```
## Tokenizing your designs
## The five groups
## Where this breaks down
## Shipping it
```

Avoid the generic scaffolding headings: "Overview", "Key Points", "Introduction", "Summary".

## Anti-AI checklist

Run this before publishing. These are the patterns that make writing read as machine-generated.

**Cut on sight:**

- "It's not just X, it's Y" and "not only X but also Y" — state the positive claim directly. I had 8 of these across my posts, mostly in the 2024 one.
- "Let's dive in", "Let's explore", "Let's take a look" — start with the point.
- "It's worth noting that", "Importantly", "Interestingly", "Notably" — just say the thing.
- "In this article we will..." — the analogy is the opening.
- Rhetorical question openers: "But what does this mean for developers?" If I know the answer, say it.
- Hook teasers: "The catch?", "Here's the thing.", "Plot twist:"
- Moreover / Furthermore / Additionally — use "and", "also", or nothing.
- Significance inflation: crucial, vital, essential, pivotal, powerful, paramount.
- Hedge stacking: "could potentially", "may eventually".
- "Experts say", "studies show" with no source. Name it or drop it.

**Structural tells:**

- Em dashes: keep them rare. One or two per post, not per paragraph.
- Rule of three everywhere — "fast, reliable, and scalable". Vary the groupings, use two or four sometimes.
- Uniform paragraph length — deliberately mix one-sentence paragraphs with longer ones.
- Bullet lists of bare noun phrases — convert to prose.
- Bold overuse — bold key terms on first use only, not for emphasis mid-paragraph.
- Title Case Headings — sentence case only.
- Synonym cycling — "developers... engineers... practitioners... builders". Repeat the clearest word.

**Two tests worth running:**

1. **Paragraph reshuffle:** can two body paragraphs swap without breaking the post? If yes, it's a list of points, not an argument. Each section should depend on the one before.
2. **What's new here?** For every paragraph, name the one fact or turn it adds. If there isn't one, cut it.

## Writers whose approach matches mine

Useful when I need a reference point for a specific move, not people to imitate wholesale.

- **[Julia Evans](https://jvns.ca/)** — plain vocabulary, concrete examples, openly says what she hasn't figured out. Closest match for the vocabulary rule and the self-deprecating register.
- **[Josh Comeau](https://www.joshwcomeau.com/)** — sustained analogy plus incremental code build-up. The two structural moves I use most.
- **[Brad Frost](https://bradfrost.com/blog/)** — the atomic design source. Analogy-driven and opinionated with real carve-outs.
- **[Nathan Curtis](https://medium.com/@nathanacurtis)** — design system naming and structure, grounded in practitioner detail. Already cited in my token post.
- **[Amy Hupe](https://amyhupe.co.uk/articles/)** — design systems in plain English, strong stances that name their own limits.
- **[Dan Abramov](https://overreacted.io/)** — "here's my position, and here's exactly where it doesn't hold" as a default mode.

## Quick pre-publish pass

1. Does it open on the analogy, with no throat-clearing?
2. Is there exactly one analogy, and does it appear in every section?
3. Is "you" the dominant pronoun?
4. Does the code build up in order, each block with a lead-in?
5. Is there at least one clear stance with an honest carve-out?
6. Do I admit at least one mistake of my own?
7. Does the ending return to the analogy, then recap briefly?
8. Are there short sentences doing real work? Any paragraph of only one sentence?
9. Anti-AI checklist clean?
10. `npm run format:fix` run?
