# Writing style — Dennis (Dipsaus)

This is my personal writing style reference. Every new post follows it. It was built by measuring my own published posts, not from a generic template. Most of the numbers below are what I already do. A few are deliberate corrections to it, and those say so where they appear.

Read this before drafting or reviewing anything in this repo.

## The one-line version

I explain an abstract frontend idea by running a single real-world analogy through the whole post, talking directly to the reader as "you", building the code up piece by piece, and taking a clear stance that admits where it breaks. Clauses join on words that name a relationship, not on `and`.

## Voice fingerprint

Measured across my four published posts (9,447 words), except the paragraph rows, which are a correction to them. The clause-join row is a measurement too, but of the older four only — my recent posts fail it. These are targets, not trivia. A draft that misses them badly is not in my voice.

| Marker                     | Target                   | Why                                                                                                                      |
| -------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| Sentences per paragraph    | 4.5–6 average            | A paragraph carries a piece of the story. Three sentences is a statement, not an argument.                               |
| Paragraph length spread    | standard deviation ≥ 1.5 | This is the one that matters. My posts run 1.5–2.4. Uniform paragraphs read mechanically even when the average is right. |
| Most common paragraph size | under 35% of paragraphs  | If half the post is 5-sentence paragraphs, the rhythm is machine-made. Spread across 2 to 10.                            |
| Mean sentence length       | 14–17 words              | My 2023 posts sat at 12.5–15.3. The 2024 post drifted to 19.9 and reads flatter.                                         |
| Sentences under 8 words    | under 10% of sentences   | The punch lines. They land inside a paragraph, never as one. A run of them reads like slogans.                           |
| One-sentence paragraphs    | 0–2 in a whole post      | Isolating a line says "this matters". Do it six times and it says nothing.                                               |
| Sentences over 30 words    | under 5%                 | If it needs 30 words, it's two sentences.                                                                                |
| Clauses joined with `, and/but/so` | under 15% of sentences | My four design-system posts sit at 13%. The two AI posts drifted to 26% and 34%, and that is what makes them hard work. |
| "you" / "your"             | 18–27 per 1k words       | The dominant pronoun. I'm teaching, not narrating.                                                                       |
| "I"                        | 2–4 per 1k words         | Present, but not the subject. It shows up for opinions and mistakes.                                                     |
| Analogy hits               | 4–9 per 1k words         | The signature move. `from-atoms-to-excellence` hit 9.2 and it's my most distinctive post.                                |
| Contractions               | 5–8 per 1k words         | Conversational, not chatty.                                                                                              |
| Ellipses (`...`)           | zero                     | I've never used one. Don't start.                                                                                        |
| Hedges vs assertions       | assertions ~2.5x hedges  | Opinionated.                                                                                                             |

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

Build a longer explanatory sentence, then cut it off with a very short one. The contrast is the effect, and it happens inside the paragraph, not by breaking one out.

> A design token is a decision with a name, and the whole point is that the value underneath it can move without the name moving with it. That's the contract. Break it and your team stops trusting the system.

## How clauses join

This is the rule I was missing for a long time, and it decides more than any length target on this page. Two sentences can hit every number in the fingerprint and still be exhausting to read, because length is not what tells a reader how ideas connect. The join is.

`and` states no relationship. It sets two things side by side and leaves you to work out why they are next to each other. `because`, `when`, `while`, `if`, `since` and `so that` all name the relationship, so the reader gets the logic for free instead of reconstructing it.

> Skill-arm refactors are bigger jobs, and the limit was killing the arm I was trying to measure.
>
> Because skill-arm refactors are bigger jobs, the limit was killing the arm I was trying to measure.

Same facts, same length, and only the second one has an argument in it.

**The test:** if `and` could be swapped for `because`, `when`, `while` or `so that` without changing the meaning, swap it. If none of them fit, the two clauses are unrelated and belong in separate sentences.

**Never two coordinating joins in one sentence.** Three clauses chained on commas and `and` is a run-on whatever its word count, because nothing in it says which idea depends on which:

> Your rules come first, long before any test exists, and they're usually the arguments you've already been having in code review, and mine are three.

Front-load the circumstance instead and let the main clause land at the end:

> Long before any test exists, you already have the rules. They are the arguments you have been having in code review for years. Mine come down to three.

This is also the fix for a draft that reads flat after an editing pass. Merging short sentences with `, and` lowers the choppiness numbers while making the prose harder to follow, which is the trap: the metrics improve and the writing gets worse.

## Vary how sentences open

Never three sentences in a row opening on the same kind of subject. A run of `The...`, `You...`, `It...` flattens a paragraph even when every sentence in it is well built, and it is the fastest tell that a draft was assembled rather than written.

The fix is the move above. Put the circumstance, the condition or the timing first, then let the main clause land:

> The limit was killing the arm I was trying to measure.
>
> Because skill-arm refactors are bigger jobs, the limit was killing the arm I was trying to measure.
>
> Long before any test exists, you already have the rules.

Do not count these. Read the paragraph aloud and listen for the three-in-a-row.

## Paragraph rhythm

Write paragraphs the way a book does. A paragraph carries a piece of the story, so it needs room to set something up, show it happening and land it. Most of mine should run five or six sentences, and the good ones run longer than that. This is the one place in this document where the numbers are not a measurement of my published posts. They are a correction, because my old posts fragment more than I want them to.

The failure I keep falling into is not choppy sentences. It is choppy paragraphs: a run of one-line declarations stacked down the page, each sitting on its own with white space around it.

> Nobody had checked.
>
> The knowledge that is hardest to hand over is the knowledge you forgot you had.
>
> You cannot ask it whether it understood.

Every one of those is a decent sentence and the sequence is exhausting. Putting a line on its own is how you tell the reader it matters, so doing it six times in an article tells them nothing at all. It also reads like a conference slide or a LinkedIn post, which is the register I am trying hardest to stay out of. The short line still belongs in the post. It belongs inside the paragraph, where the sentences around it earn it, and the standalone version gets saved for a genuine turn in the argument once or twice per article.

The test is whether the paragraph is doing something. Walking through what happened, building a case, working an example, admitting where it went wrong. If it is one assertion in a box, it is a slogan, and it should be folded into the paragraph above or below it.

The other way to get this wrong is uniformity. You notice the problem, you start writing everything at five sentences, and the result is more mechanical than what you started with, because no human thinks in evenly sized units. So check the spread, not the average. Paragraphs of two and paragraphs of ten belong in the same post, and if one length accounts for more than a third of it the rhythm has gone flat.

## Every paragraph earns its place

Cut anything that does not carry weight. This matters more than any metric on this page.

For every paragraph, name the one thing it adds: a fact, a turn in the argument, an objection, a concrete example. If you cannot name it, the paragraph is restating the one before it in fresh words, which is the single most common way a draft gets longer without getting better. Delete it and the post improves.

The same test applies inside a sentence. A clause that only softens, or re-announces what is coming, or tells the reader that something is interesting instead of being interesting, is padding. Cut the frame and keep the content.

Short is not the goal. Dense is. A 2,500-word post where every paragraph moves is a better read than a 1,200-word one padded with transitions.

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
- Chained coordination — two `, and` joins carrying three clauses in one sentence. Split it, or subordinate one clause to another.
- Subject-first runs — three sentences in a row opening on `The`, `You` or `It`.
- Statement stacking — several one-line paragraphs in a row, each landing a point. Fold them into the paragraphs around them.
- Uniform paragraph length — mix short paragraphs with long ones, but do the mixing at two sentences and up.
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
8. Do the paragraphs tell a story rather than stack statements? No more than two standalone one-sentence paragraphs?
9. Does every `, and` name a real relationship, or should it be `because` / `when` / `while` / a full stop? Any sentence with two of them?
10. Read aloud: any three sentences in a row opening the same way?
11. Anti-AI checklist clean?
12. `npm run format:fix` run?
