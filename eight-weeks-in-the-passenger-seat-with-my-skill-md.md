---
title: 'Eight weeks in the passenger seat with my SKILL.md'
date: 2026-08-27
tags: [ai, claude-code, react, evals, skill-md-series]
slug: eight-weeks-in-the-passenger-seat-with-my-skill-md
---

# Eight weeks in the passenger seat with my SKILL.md

Passing the theory exam does not get you a driving licence. You answer the questions about mirrors and roundabouts, you collect a certificate, and then somebody still has to sit next to you in the car. The examiner watches your hands the whole way. That is the half that decides whether you can drive. A SKILL.md is a theory paper. You write down how your code should look, the model reads it, and everyone assumes the practical went fine. Nobody sits in the passenger seat.

Right now every article and every conference talk lands on the same advice. Write better prompts. Give the model context. Hand it a skill. So we do. We install skills, we write skills, and we pollute the context with files nobody has ever checked. We have become careful about the prompt and careless about everything we staple to it.

As I have some experience in the field of development I should know what good React looks like, which is the only reason I can be the examiner at all, and it should mean I can hand that knowledge to a model. In [the first post](https://www.divotion.com/blog/you-can-t-put-a-career-in-a-skill-md) I argued that you cannot put a career into a SKILL.md, and I still think that. Writing one down does not make it useful. It makes it a claim. What I wanted to know was where the gaps sit: which of my rules survive the trip into a file, which ones the model already knew without me, and which ones do nothing at all. So I spent eight weeks building an examiner for one file of my own. What follows is that build in the order it happened, wrong turns included, because the two versions that died explain the third one better than the third one does on its own. This post is what it cost, what it caught, and where it failed, so the next developer does not have to run the same experiment to find out which parts work.

## Table of contents

- [The standard came out of an interview](#the-standard-came-out-of-an-interview)
- [Two questions, two price tags](#two-questions-two-price-tags)
- [Three ways to ask](#three-ways-to-ask)
- [What the clean run said](#what-the-clean-run-said)
- [The fixtures that taught me the most](#the-fixtures-that-taught-me-the-most)
- [Deterministic output from a non-deterministic machine](#deterministic-output-from-a-non-deterministic-machine)
- [What eight weeks actually bought](#what-eight-weeks-actually-bought)

## The standard came out of an interview

An experiment needs a subject. I had no React architecture skill worth testing, so writing one became step one. I did not sit down and write it. I ran a custom interview skill, inspired by `grill-me`, and let Claude interview me about the React decisions I actually care about. I brought the patterns, Claude wrote them out, and I pushed back on the examples until they said what I mean in a review. It took several rounds. React architecture was the subject for one reason: it is the code I can still mark by hand, and until the examiner is automated, marking by hand is the only ground truth there is.

What came back is 156 lines and twenty rules, and every rule carries a stable id. The ids sort into four families:

- `srp.*` for single responsibility, including the hard caps: 150 lines, 5 hooks, 6 props, 2 effects, JSX depth 5.
- `boundary.*` for feature boundaries, the same idea scaled up from a component to a folder.
- `comp.*` for compound components and composition, where regions are slots and a discriminator prop is a smell.
- `state.*` for state and data boundaries, covering server fetching, derived state, colocation and global stores.

The ids do more work than anything else in that file, and I would not have thought to ask for them without the interview. "This component does too much" is an opinion. `srp.mixed-concerns` on `Bad.tsx` is a claim you can score, and every number later in this post exists because of that one change.

The prefixes earn their place for a different reason. Twenty rules is too many to read a report about, while four families is a column you can scan. So when I change the standard, I can see whether composition slipped or state did, instead of comparing twenty near-identical lines. That only holds if the model actually returns the ids, and here I have to be careful about what I know. I ran the whole corpus against one model, and Sonnet gives back the id verbatim in almost every call. Whether a smaller or older model holds a twenty-token vocabulary that reliably is a question I have not paid for yet.

```md
| Id                     | Severity | Rule                                                                      |
| ---------------------- | -------- | ------------------------------------------------------------------------- |
| `srp.loc-cap`          | high     | ≤ 150 lines of code per component                                         |
| `comp.config-soup`     | med      | compound API + context when config flags, > 6 props, or shared part state |
| `state.derived-effect` | high     | no derived state in `useEffect`; compute during render                    |
```

Severity tracks reversibility rather than annoyance. High is for what creates bugs or coupling that costs real money to unwind, while med is for API-shape smells you can refactor later. There is no third level, and that was a deliberate cut. My first draft had a `low` tier, and every rule that ended up in it was a rule I would never actually raise in a review. A standard carrying advice nobody acts on teaches the model that some of its instructions are optional. The low tier went, and the four rules sitting in it went with it.

## Two questions, two price tags

Then I had a standard and no idea how to test it. There is no framework for this, so I reached for the shape I already knew: a test suite, green or red, like everything else in CI. Two versions of it died before I understood why. Both are worth walking through, because the reason they died is the reason the third one is built the way it is.

The first version asked a model to score each refactor from 0 to 100. On identical work that score swung between 5 and 97, so I deleted the judge and replaced it with binary pass/fail checks. Those flaked too, so I ran each fixture three times and took the majority vote. Majority voting did not rescue pass/fail either. A hard threshold on a stochastic refactor cannot be stable, because near the bar ordinary run-to-run variance flips the build, and a majority of three does nothing when a check sits at a coin flip. An examiner is allowed to say pass or fail because you sit the test once, on one Tuesday, in one car. I was grading something that answers differently every time you ask it, so a pass mark was never going to hold still. Three weeks in, I stopped gating and started measuring instead. That reframe splits the work in two, and only one half is free.

Before the two halves make sense, the thing doing the grading needs a name, because from here on there are two pieces of software in this story and only one of them is the SKILL.md.

> The **harness** is everything wrapped around the model: the runner that copies a fixture into a sandbox, the prompt builder, the parser that reads the answer back, and the grader that measures the result.

Nothing exotic sits behind that word. A folder of deliberately broken React components, a shell script that starts a headless `claude -p` for each one, and a grader that reads the answer against a key I wrote by hand. If you have ever run a test suite you already have the picture, and the only real difference is that one step in the middle gives a different answer every time you run it. That step is the model, and it is the only part that costs anything to run.

**The cheap question is whether my test code works.** Every part of the harness that can be checked without a model gets checked without a model: the parser that turns a review into structured findings, the matcher that decides whether a finding counts as a hit, and the grader that measures a component with the TypeScript compiler API. Plain unit tests, running in CI on every push. Free and deterministic, 88 cases across nine files.

Concretely, the grader has to agree with me about what a component contains before it is allowed to judge one. Feed it a component with seven props, seven hooks, three effects and six levels of JSX, and it has to say so:

```ts
const [metrics] = measureComponents('Overloaded.tsx', OVER_CAPS)
expect(metrics).toMatchObject({ props: 7, hooks: 7, effects: 3, jsxDepth: 6 })
```

**The expensive question is whether the skill works.** For that I keep 25 fixtures. Every broken component is React I have actually met, the shape of code that turns up in a pull request on an ordinary Tuesday. If a junior added it to our codebase I would leave exactly the review the fixture expects. That is the bar I wanted: a standard that cannot catch the comments I have already written a hundred times is not worth loading into anybody's context. Each rule gets its own folder, one manoeuvre on the test route, and the fullest of them hold three files.

- `Bad.tsx` is the broken component.
- `Demo.tsx` uses it, so an ordinary unit test can pin down what the component does today.
- `Good.tsx` is the same job written the way I want it.

Not every rule needs all three. `Demo.tsx` turns up in nine of the twenty-five, the fixtures where the refactor is expected to change the component's public API and something has to hold the rendered output still while it does. The model never sees `Good.tsx` in any of them. That leaves the examiner three things to check, which between them are the whole exam. It has to pinpoint the problems in `Bad.tsx`. It has to find nothing in `Good.tsx`, because a finding against healthy code is a false positive, and a skill that shouts about clean code is its own kind of failure. And it has to turn `Bad.tsx` into something that ends up looking like `Good.tsx` without ever having read it. If the unit test still passes afterwards and the result matches what I would have written myself, I count that as a success.

![One fixture folder holding Bad.tsx, Demo.tsx and Good.tsx. Bad and Demo cross the line to a headless claude -p; Good never does. Three checks come back: two review checks, find every violation in Bad.tsx and report nothing in Good.tsx, and one apply check, refactor Bad.tsx until the behaviour test passes and the result matches Good.tsx](images/eight-weeks-in-the-passenger-seat-with-my-skill-md/fixture-anatomy.jpeg)

Then I run Claude against them headlessly, five times per fixture. One run is an anecdote. Five is a rate. This is the whole of `state/derived-effect/Bad.tsx`, the smallest fixture I have:

```tsx
export function SearchSummary({ items, query }: { items: string[]; query: string }) {
  const [matchCount, setMatchCount] = useState(0)

  useEffect(() => {
    setMatchCount(items.filter((item) => item.toLowerCase().includes(query.toLowerCase())).length)
  }, [items, query])

  return (
    <p role="status">
      {matchCount} of {items.length} products match “{query}”
    </p>
  )
}
```

Next to it sits the answer key. `expected` is what a correct review must report. `alsoAcceptable` is the overlap I refuse to punish, because several of my rules genuinely fire on the same line.

```json
{
  "files": {
    "Bad.tsx": {
      "expected": [{ "rule": "comp.config-soup", "line": 1 }],
      "alsoAcceptable": ["comp.regions-as-slots", "comp.slots-over-config", "comp.variant-compound"]
    },
    "Good.tsx": { "expected": [], "alsoAcceptable": [] }
  }
}
```

## Three ways to ask

Every call has the same shape. One headless `claude -p`, with the skill pushed in as a system-prompt append rather than through plugin resolution, so the same bytes reach the model on every machine and in CI.

```bash
claude -p --output-format text \
  --model claude-sonnet-5 \
  --append-system-prompt "$(cat skills/react-architecture/SKILL.md)"
```

The fixture itself goes in on stdin, and not because that is elegant. Variadic flags like `--allowedTools` will happily swallow a positional prompt argument, and it takes an afternoon to work out why the model is answering a question you did not ask. Wrapped around the files is the only instruction that really matters, the one that makes the answer parseable:

```text
Review the following React/TypeScript files against the skill's standards.
Output ONLY the review-mode findings report in the skill's format — every
finding must carry its stable rule id, and file paths must be exactly the
paths given here. Do not add prose before or after the report.
If nothing violates the standards, output exactly: NO_FINDINGS

### File: Bad.tsx

<the whole file, fenced>
```

**Review mode** is the theory paper. It asks the model to find the violations and scores the hits. It grades on rule and file, never on line number. Models find the right rule in the right file and then anchor the line wherever they like, so the labelled line survives as documentation for humans and nothing else.

**Apply mode** is the hill start, the manoeuvre on the practical where describing it earns you nothing, because the car either holds on the slope or it does not. Its prompt is an instruction rather than a question, and it carries every constraint that makes the grade mean anything:

```text
Refactor the files in the current working directory in place so they
satisfy the architecture standards from your instructions: Bad.tsx, Demo.tsx.
Use your file tools to edit them. Preserve behavior — behavior.test.tsx
must keep passing and MUST NOT be modified. If a Demo.tsx exists, it is
the caller the tests exercise: update its usage to your new API so its
rendered output stays identical. Keep TypeScript strict-clean.
You may create new files (extracted hooks/components) in this directory.
```

That runs in a sandbox with real file tools, and the result is graded by the AST cap checks, `tsc`, and the fixture's own behaviour test. The runner restores the sandbox from a pristine copy first, so the model cannot quietly edit the test and grade itself green.

**A/B mode** runs everything twice, once with the skill loaded and once with a neutral reviewer prompt. That control arm was harder to write than the skill. Sending nothing at all is the obvious control and the wrong one, because a blank sheet is a different exam. The control gets the identical output format, the same twenty rule ids, the same escape hatch, and none of the caps, severities or criteria. It took two rewrites.

Every run diffs against a committed baseline, so a rate drop fails the run and names the rule and the fixture that slipped. One practical note on living with a corpus like this: the fixtures deliberately violate my own lint rules, so I fence that directory off from CI three separate ways: the root `tsconfig.json` excludes `tests/eval`, the oxlint `ignorePatterns` covers `fixtures`, and CI's test command runs the `unit` vitest project only.

## What the clean run said

Six commits went into the standard over those eight weeks, and seventy went into the thing that grades it. That ratio is the honest summary of the experiment: most weeks I was not editing my React opinions, I was fixing the exam. One of those weeks I spent measuring whether a language model can read a comment, because eighteen `Bad.tsx` files still carried the header comment that named the rule they violated and it shipped verbatim to the model in every call. The clean run came after all of that, five runs per fixture, one model, both arms on the same corpus. This is the mark sheet.

| Category          | Detection (skill / control) | Apply pass (skill / control) |
| ----------------- | --------------------------- | ---------------------------- |
| composition       | **91% / 77%**               | **80% / 0%**                 |
| srp               | 95% / 88%                   | 97% / 97%                    |
| state             | 88% / 93%                   | 100% / 97%                   |
| hard (multi-file) | 100% / 97%                  | 50% / 70%                    |

The composition column is the answer to the whole experiment. Per fixture, the control arm scored 0/5 on config-soup, 0/5 on dashboard-panel, 0/5 on regions-as-slots, 0/5 on slots-over-config and 0/5 on variant-compound. A capable model, reviewing against its own judgement of good React, never produces a compound component. Not rarely. Never. That gap is why the skill exists.

I am keeping the two rows that do not flatter it. State detection dips with the skill, 93% down to 88%, and I have no good explanation. Hard-tier apply drops to 50%. One fixture drives that, where the judge failed the skill arm's refactors over a `compact` boolean that only changes styling. That one is my fault, not the model's. I had written a rubric criterion the skill never taught. Twelve control runs also died mid-run on a session usage limit and I replaced them from filtered re-runs, a note that lives in the report's own `repairs` field and not only in this post.

## The fixtures that taught me the most

**The one that needed no skill at all.** `state/derived-effect` scores 5/5 in both arms. Every model already knows that mirroring a computed value into `useState` is wrong. The skill adds nothing there, and knowing which rules are decoration is worth as much as knowing which ones work.

**The one I cannot fix.** `state.colocate` sits at 2/5, in both arms, and has since the first baseline. The fixture puts the draft state in the page while only the composer reads it:

```tsx
export function SupportPage({ onSend }: { onSend: (message: string) => void }) {
  const [messageDraft, setMessageDraft] = useState('')

  return (
    <main className="support-page">
      <h2>Contact support</h2>
      <MessageComposer draft={messageDraft} onDraftChange={setMessageDraft} onSend={onSend} />
    </main>
  )
}
```

Look at it honestly. That is a controlled component with lifted state, which is a pattern every React codebase uses on purpose. The violation only exists because I know nothing else will ever read `messageDraft`, and that fact is not in the file. From the passenger seat, the driver did nothing wrong. I cannot build a fixture where the answer is obvious to me and the evidence is invisible to the reader. That is a real limit on this whole method. It only pays for rules you can seed into a file and grade on the way out.

**The one that nearly passed.** On `composition/dashboard-panel`, the model built the compound API exactly as asked, exporting `Header`, `Subtitle`, `Body` and `Footer`. Then it moved the flag into context and gated the parts on it. Every mechanical gate passed: originals untouched, caps fine, no banned pattern, `tsc` clean, behaviour tests green. The judge failed it and described it better than I would have:

```text
This is show/hide flag logic just relocated behind context instead of
props — same soup, moved bowl.
```

**The one change that paid.** The single most effective change I made to the standard was fourteen lines. A before and after, showing that a discriminator prop must die rather than get renamed. That moved `variant-compound` apply from 3/5 to 5/5 and `dashboard-panel` from 3/5 to 4/5. Later, fixing one rubric sentence took `slots-over-config` detection from 2/5 to 5/5 and its apply from 3/5 to 5/5. One worked example beat every abstract rule I had written about the same thing.

## Deterministic output from a non-deterministic machine

That heading is the goal I wrote down in week one, and it is the one thing on this page I did not get. The machine stays random, and no amount of prompting or repetition talks it out of that. What you can pin down is everything around it: a rate instead of a verdict, a committed baseline to hold that rate against, and a build that fails and names the fixture when it slips. That is not the experiment failing. It is the result, and it changed how I grade anything a model produces.

Today the baseline reads 177 of 185 on detection, with 33 of the 37 rule-and-fixture combinations clean in every single run. Apply sits at 121 of 125 through the gates and the judge. When I edit the standard now, I know by lunchtime whether I made it worse. It is all public too, because a claim like that is worth nothing if you cannot check it. The repo is [dipsaus9/dipsaus-ai](https://github.com/dipsaus9/dipsaus-ai): the skill in `skills/react-architecture/SKILL.md`, the corpus in `tests/eval/fixtures`, the baselines in `tests/eval/baseline`, both A/B reports in `tests/eval/ab`, and the reasoning behind every fix, failures included, in `backlog/tasks`.

## What eight weeks actually bought

The honest verdict after eight weeks is that testing a skill works, that it helps, and that it is nowhere near perfect. It also does not move the argument I made in the first post one inch. Measuring a standard makes it trustworthy, not bigger: twenty rules with a baseline behind them are still twenty rules, and my job has never been twenty rules. Knowing exactly which of them land is worth a great deal, and it is still not a career in a file.

Which is why the next thing I changed was not the file. Partway through this experiment I stopped trying to make one skill carry more and started changing how I work with the model instead, orchestrating several agents with smaller jobs around the delivery of real work rather than pushing everything through a single document. That is the next post.

The theory paper is the easy half. Eight weeks in the passenger seat taught me which of my rules survive the trip into a markdown file, and that the ones which do not are not the model's fault. You cannot hand over a career. You can hand over twenty rules and know, to a number, which of them arrive.

---

_Part 2 of three on writing down how you work and then finding out whether it landed. Part 1: [You can't put a career in a SKILL.md](https://www.divotion.com/blog/you-can-t-put-a-career-in-a-skill-md). Next: **I stopped writing skills and started drawing plans**._

<!-- TODO: link part 3 once it is published -->
