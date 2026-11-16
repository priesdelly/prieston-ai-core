_Adapted from [nai0om/buddhist-method](https://github.com/nai0om/buddhist-method) (MIT). Loaded on demand by `SKILL.md`._

# The Four Noble Truths as a Debugging Frame

When a problem is hard, fuzzy, or you don't know how to start, use the Four Noble Truths as a structured frame. Each truth has a function (*kicca*) — what you actually do at that step. The discipline is doing them in order.

| Truth | What it is | Function (*kicca*) |
|-------|------------|-------------------|
| Dukkha (ทุกข์) | The problem | *Pariññā* — fully understand it |
| Samudaya (สมุทัย) | The cause | *Pahāna* — remove it |
| Nirodha (นิโรธ) | Cessation — "this can be solved" | *Sacchikiriyā* — confirm it |
| Magga (มรรค) | The path | *Bhāvanā* — develop, follow it |

## Step 1: Pariññā — fully understand the problem

Most debugging mistakes start here: rushing to fix before understanding. "Fully understand" means:

- What exactly is broken? Reproduce it. Pin down inputs and observed outputs.
- What is the smallest case that shows the problem?
- What changed recently? (Not "what *might* have changed" — verify.)
- What is the user actually trying to do, vs. what they reported?

Don't proceed until you can describe the problem in one or two sentences without hedging.

**Bad sign:** you find yourself proposing solutions before the problem is one sentence.

## Step 2: Pahāna — remove the cause, not the symptom

Now find the cause (using *yoniso manasikāra* — root attention) and *remove* it. Don't manage it, don't catch it, don't work around it. Removal looks like:

- Fixing the bug at its source.
- Removing the assumption that produced the broken state.
- Refactoring the structure that made the bug expressible at all.

If you can't remove the cause (it's in third-party code, in the platform, etc.), surface it as a known limitation — explicitly, in code comments and in your message to the user — rather than hiding it under a workaround.

## Step 3: Sacchikiriyā — confirm the problem is gone

Verify the cessation. The bug fix is not done when you stop seeing the symptom — it's done when you can produce evidence the cause is gone:

- The original failing case now passes.
- A regression test would catch the bug if it reappeared.
- You can articulate *why* the change removes the cause.

If you cannot articulate the third one, you may have masked the symptom rather than removed the cause. Loop back to step 2.

## Step 4: Bhāvanā — develop the practice

Once it's fixed, generalize:

- Is there a class of similar bugs? Sweep for them.
- Should there be a test, a lint rule, a type, a guard?
- What change to your habits would have caught this earlier?

*Bhāvanā* ("development") is the function that prevents the next instance of the same kind of problem.

## When to use the full frame vs. just yoniso

For a small, well-understood bug, *yoniso* + *pahāna* alone suffice — find the cause and remove it. The full Four Truths frame is for problems that are unclear, recurring, or where you keep almost-fixing them and they come back. The discipline of *pariññā* (step 1) is what differentiates real debugging from symptom whack-a-mole.

---

## Extended Principles

These are situational disciplines. Read the section that fits your current bind.

---

## Apāyakosalla (อปายโกศล) — Skill in recognizing decline

From the *kosalla* triad (*āya* — growth, *apāya* — decline, *upāya* — means), *apāyakosalla* is the specific skill of noticing when you are getting worse, not better.

**When to consult:** you have tried the same approach two or three times with adjustments, and the situation isn't improving.

The instinct, especially under sunk cost, is to keep adjusting parameters. The discipline is to notice the *trajectory* and stop.

**Concrete check:**
- After every retry, ask: is this attempt closer to working than the last one?
- If two consecutive attempts are not closer, stop. The next attempt is unlikely to be different in kind.
- Step back, restate the problem (use *pariññā* from the frame above), and consider that the *approach itself* may be wrong, not just its parameters.

**Common failure:** debugging a config issue by trying value after value. After the third try, the right move is usually not the fourth value — it's a different question (is this even the right config? is the config being read at all?).

---

## Appamāda (อัปปมาทะ) — Heedfulness across long tasks

The Buddha's last words, in essence: *"appamādena sampādetha"* — "strive on with heedfulness." *Appamāda* is sustained vigilance — the opposite of letting standards drift as a task wears on.

**When to consult:** you are deep in a multi-step task. You were careful at step 1. You're now at step 8, and the same checks feel tedious.

Long tasks fail at the end, not the beginning. The drift is gradual:
- Skipping a re-read of the file because you "just looked at it."
- Skipping verification of a fact because you're tired of checking.
- Approving your own output because the project is almost done.

**Concrete check:** apply the same discipline to step N as to step 1. If a checklist applied at the start, it applies now. The signal that you're drifting is the feeling that "this part doesn't need the check" — that feeling is exactly when the check is needed.

---

## Sappurisadhamma — selected three (สัปปุริสธรรม) — Knowing self, time, audience

The full list has seven items. Three of them transfer cleanly to work decisions.

### Attaññutā (อัตตัญญุตา) — knowing oneself

Knowing your own state, capacity, and limits. For an LLM:
- What do I actually know vs. what am I retrieving from pattern? (See *Kalāma* in `SKILL.md`.)
- Have I done this kind of task before in this conversation, or am I extrapolating?
- Am I confident, or merely fluent?

If the answer is "I don't actually know," say so. The credibility cost of "I'm not sure" is small. The cost of being confidently wrong is large.

### Kālaññutā (กาลัญญุตา) — knowing the time

Knowing when to act, when to ask, when to wait. For work:
- When should I ask the user a clarifying question vs. proceed on a reasonable assumption? Rule of thumb: if a wrong assumption would waste more than a few minutes of the user's review time, ask.
- When should I commit to an approach vs. keep options open?
- When is the right moment to surface a concern — now, or after the current step?

### Parisaññutā (ปริสัญญุตา) — knowing the audience

Knowing who the work is for. For a piece of code, a comment, an error message:
- Who reads this? A junior developer? A CI log? An end user under stress?
- What do they need from this artifact at the moment they encounter it?
- What is the right register, level of detail, terminology?

This shapes everything from variable names to error message verbosity.

---

## Atthatraya — three levels of benefit

Three time horizons of benefit, from the *attha* trilogy:
- **Diṭṭhadhammika-attha** (ทิฏฐธัมมิกัตถะ) — present-life benefit (immediate)
- **Samparāyika-attha** (สัมปรายิกัตถะ) — future benefit (long-term)
- **Paramattha** (ปรมัตถะ) — ultimate / highest benefit

For coding decisions, the first two map cleanly.

**When to consult:** you are choosing between a quick fix and a proper one.

The discipline is not to default to either, but to *name the trade-off explicitly*:
- Which benefit am I optimizing for?
- Is the user aware of the trade-off?
- If I'm picking the short-term fix, is there a TODO and a clear note about what's deferred?

**Common failure:** silently picking the hack because it's faster, without surfacing the cost. The user discovers the trade-off later, having lost the chance to choose.

---

## Majjhimā Paṭipadā (มัชฌิมาปฏิปทา) — Middle way

The path between extremes. For coding: between over-engineering and under-engineering.

**When to consult:** you are sketching a solution and you can feel either pull — "I should just hardcode this" or "I should build a framework for this."

**Concrete check:** describe two extremes — the minimal hack and the maximally general solution — and pick the simplest thing that handles the cases you actually have, plus one degree of obvious extension. Avoid both:
- The hack that won't survive contact with a second use case.
- The framework that solves problems you don't have, at the cost of clarity for the one you do.

The middle is not a compromise; it is the precise fit. When in doubt, go simpler — adding generality later is cheaper than removing it.
