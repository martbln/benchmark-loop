# The blind judge protocol

The judge is the only thing standing between the work and self-flattery. Run it properly or the whole loop is theater.

## Contents
1. Setting up a valid comparison
2. The judge prompt
3. Verdict format
4. Calibrating harshness
5. When there's no reference to compare against
6. Common ways the judge gets corrupted

---

## 1. Setting up a valid comparison

**Match the framing.** The two artifacts must be comparable on the dimension being judged. Same shot type and camera distance for visuals. Same section type and word count for writing. Same task for a running app. Same length and instrumentation for audio. A judge given a wide landscape and a close-up will report on composition, which tells you nothing about your materials.

**Strip identity.** Remove watermarks, UI chrome, filenames, metadata, house fonts, characteristic mouse cursors — anything that identifies which is the reference. If your build has a debug overlay and theirs doesn't, the judge is now grading overlays.

**Randomize order every run.** Don't always put yours second. Position bias is real and consistent.

**Fresh context.** The judge must not have built the work, seen the plan, or read earlier judge rounds. In tools with subagents, spawn a new one. Without them, start a clean conversation and paste only the two artifacts and the prompt below.

**Odd number of runs.** Three is usually enough. One run of a subjective judgment is noise.

---

## 2. The judge prompt

Adapt the dimension list and evidence rule per domain, keep the structure:

```
You're evaluating two [screenshots / documents / builds / clips], A and B.
One is from a professional reference; one is a work in progress. You are not
being told which is which, and guessing is not the task.

Judge primarily on: [ranked dimension 1], then [dimension 2].
Ignore [explicitly deprioritized dimensions] for now — they're not what's
being decided here.

Answer in this order:
1. Verdict: A or B is better on the primary dimensions, or "indistinguishable."
2. Evidence: three specific observations that decided it. Point at concrete
   things you can actually see in the artifact — a region, a sentence, a
   moment, a value. Not "looks more polished."
3. Flip condition: name the single change to the losing one that would most
   move your verdict.

If you cannot name a concrete flip condition, say so — that means the two are
at parity on what you were asked to judge.
```

That last clause is load-bearing. It gives the loop a natural termination that doesn't depend on anyone deciding to be satisfied.

---

## 3. Verdict format

Log every round so drift is visible:

```
Round 3 | component: lighting | runs: 3
Verdict: reference (2), ours (1)
Recurring gap: no contact shadows where geometry meets floor
Flip condition: add contact-shadow term / AO at intersections
Dimension drift: none (still on primary)
```

The **dimension drift** line matters. When the judge stops talking about lighting and starts talking about font choice, you've hit the ceiling on the thing you were fixing — that's a stop signal, not a new task.

---

## 4. Calibrating harshness

"Be a really harsh critic" is directionally right — a lenient judge is useless — but taken literally it produces an infinite fault generator, because any artifact has infinite faults at sufficient zoom.

Bound it two ways:

- **Scoped harshness:** brutal on the ranked primary dimensions, silent on everything else until those close. This is what keeps a run from spending three loops on a menu font while the core still feels wrong.
- **Evidence requirement:** every criticism cites something observable in the artifact. This kills the generic critique reflex ("lacks depth", "needs refinement") that pattern-matches to sounding discerning without saying anything actionable.

A judge that must point at a pixel or a sentence can only be as harsh as reality supports.

---

## 5. When there's no reference to compare against

Some targets have no single artifact to hold up: an internal tool, a novel research question, a first-of-its-kind product.

Build a **composite rubric** instead. Take three exemplars that are each excellent on one dimension, extract what specifically makes each good on that dimension, and judge against the assembled criteria. It's weaker than direct comparison — the judge scores rather than chooses, which reintroduces some leniency — so compensate by making each criterion concrete enough to fail: "every claim traces to a primary source" rather than "well-sourced."

Where a partial reference exists, prefer direct comparison on that part and rubric on the rest.

---

## 6. Common ways the judge gets corrupted

- **The builder judges.** Even with instructions to be harsh. It knows which one it made.
- **The judge is told the framing.** "Here's our attempt vs. the professional one" — verdict is now decided before it looks.
- **Context carries over.** Judge rounds 1–4 in one conversation means round 5 is anchored on round 1.
- **The reference is cherry-picked weak.** Comparing against the reference's worst moment to make parity easier. Feels good, proves nothing.
- **Criticism without evidence gets accepted.** "Needs more polish" enters the work order, someone polishes something arbitrary, the loop learns nothing.
- **The artifact shown isn't the artifact built.** A beauty-shot screenshot from the one good angle. Judge on representative output, not best-case output.
