---
name: benchmark-loop
description: Anchor an ambitious task to a named external benchmark, decompose it, and iterate against a blind adversarial judge until the output is indistinguishable from the reference. Use this whenever someone asks for work "at the level of" a named product, studio, publication, or paper; uses words like perfect, AAA, world-class, production-grade, publication-quality, "not a prototype", or "as good as [X]"; asks to loop, fan out, or keep going until quality is reached; or hands over a big open-ended build request (game, app, research report, deck, essay, design system, dataset) where the hard part is holding a high bar rather than knowing the steps. Also use when a first attempt came back mediocre and they want it pushed much further, or when they say "don't stop until it's great."
---

# Benchmark Loop

## The idea

Almost every "make this excellent" request fails the same way: nobody said what excellent means, so the thing that built the work also grades it — and it grades generously. "Looks good to me" is the default output of any builder inspecting its own work.

This skill removes both problems. **Borrow the quality bar from something that already exists**, then hand judging to a separate critic that doesn't know which artifact is yours. A judge that can't tell which one it built can't flatter itself. That's the whole trick; everything below is plumbing around it.

Concretely: instead of "build a great shooter," you build one level, screenshot it, screenshot the same kind of scene from a real Call of Duty, hand both to a fresh judge unlabeled, and ask which looks better and why. You get a falsifiable answer and a specific gap to close. Repeat until the judge picks yours (or genuinely can't tell).

## When to use it, and when not to

Worth it when the target is high, a credible reference exists, and quality is judgeable from an artifact — visuals, prose, a running app, a report.

Skip it when the task is small or purely functional (a script that works, works), when no reference exists in that shape (then build a rubric from three partial exemplars instead — see `references/domains.md`), or when the person needs a fast draft. This loop is expensive by design. Say so before starting a long run.

---

## Phase 0 — Set the bar

Do this before generating anything. Ten minutes here saves hours of aimless polishing.

**1. Name the reference.** Not a category — a specific artifact. "Modern Warfare II's Amsterdam level," not "AAA games." "A Stratechery post," not "good analysis." "Stripe's docs," not "clean design." If the person named one, use theirs.

**2. Pick the comparison slice.** This is the step people skip, and skipping it is why these loops sink. You are not going to match a 400-person studio's entire game. You can match one corridor, one weapon's handling, one lighting setup. Choose a slice small enough to actually reach parity on and representative enough that reaching it proves something. State the slice out loud: "we're matching one 60-second indoor firefight, not the campaign."

**3. Rank the dimensions.** List what quality means here — for a game: lighting, materials, animation, audio, feel, performance. For research: sourcing, argument, novelty, structure, prose. Then rank them and say which two carry the verdict. Unranked dimensions get equal effort, and equal effort is how a project spends six loops on font kerning while the core mechanic feels dead.

**4. Write the acceptance test now.** One sentence, checkable: "A judge shown our screenshot and a real MWII screenshot, unlabeled, picks ours or says it can't tell — three runs, at least two of them." If you can't write this sentence, the bar isn't real yet.

---

## Phase 1 — Foundation before fan-out

**Parallelize second, not first.** The original version of this pattern says "fan out sub-agents on every component immediately," and for anything with shared substrate that produces beautiful mismatched parts that don't compose: five weapons built against three different physics assumptions, six report sections with contradicting definitions.

So: identify the shared substrate first and build it *serially*, yourself. That's the render pipeline and lighting model, the data schema, the core thesis and definitions, the design tokens. Get it decided and written down. Everything downstream references it.

Cheap test for whether something is foundation: if two components would break each other by disagreeing about it, it's foundation.

---

## Phase 2 — Fan out on what's genuinely independent

Now decompose the slice into components that touch the foundation but not each other, and work them in parallel (subagents where available, sequentially where not).

Each component worker gets:
- the frozen foundation spec
- its own scope, and an explicit note of what it must not touch
- the reference artifact for its component specifically
- the ranked dimensions from Phase 0

Give each one a concrete target, not an adjective. "Match the falloff and specular response of this reference screenshot" beats "make the lighting AAA."

---

## Phase 3 — The judge loop

This is the engine. Full protocol in `references/judging.md` — read it before running the first comparison. The short version:

1. **Produce a comparable artifact.** Screenshot, rendered page, running build, exported PDF, audio clip. Match the framing to the reference: same shot type, same subject, same length, same format. Mismatched framing invalidates the comparison — a wide vista against a close-up tells you nothing.

2. **Judge blind.** Fresh context, no history of building it, labels stripped, order randomized. The judge must not be the builder and must not be told which is which.

3. **Demand a verdict plus a diff.** "A or B, and the three specific things that decided it." A critic that says "needs more polish" has wasted a cycle. A critic that says "B's shadows have no contact darkening where objects meet the floor, so everything looks like it's floating" has handed you the next task.

4. **Fix the named gaps only.** Don't wander. The judge's diff is the work order.

5. **Re-run with fresh eyes and a new randomization.** Repeat.

**Harshness is calibrated, not maximal.** "Be a really harsh critic" is right in spirit and misfires in practice — an unbounded critic invents faults forever, because there is always something. Instead: the judge is harsh about the top-ranked dimensions and explicitly ignores the rest until those are settled. And it must always name what specific change would flip its verdict. If it can't, it has nothing left to say and the loop is done.

---

## Phase 4 — Integration

Components that each pass individually still fail together. After the loop closes on the pieces, run one judge pass on the assembled whole against the reference — same blind protocol, whole-artifact framing. Seams, tonal inconsistency, and pacing only show up here.

---

## Stop conditions

Set these at Phase 0 and honor them. An unbounded "don't stop until perfect" is how a run burns a day and delivers loop 4 of an infinite series.

Stop when any of these hit:

- **Parity reached** — judge picks yours or can't tell, on the required number of runs.
- **Iteration cap** — pick a number up front (4–6 per component is usually right). Hitting it is information, not failure: report the remaining gap honestly.
- **Diminishing returns** — two consecutive loops where the judge's complaints get smaller and shift to unranked dimensions. That's the signal you've reached the ceiling of the current approach; further looping polishes the wrong thing. If the gap is still large, the fix is a different approach, not another iteration.
- **Wrong-bar detection** — the judge keeps citing something structurally unreachable (motion-captured animation, proprietary data, a decade of engine work). Say so plainly rather than looping against it. Re-scope the slice or re-scope the claim.

Report honestly at the end: which components reached parity, which didn't, and what the residual gap is. A truthful "the lighting matches, the animation doesn't and here's why" is worth more than a confident "done."

---

## Failure modes

| Failure | Symptom | Fix |
|---|---|---|
| Self-grading | Everything passes on loop 1 | Separate judge, blind, fresh context |
| Vague bar | "Make it AAA" | Named artifact + specific slice (Phase 0) |
| Premature fan-out | Components don't compose | Foundation serially first (Phase 1) |
| Nitpick spiral | Judge never satisfied, complaints shrink | Rank dimensions; cap iterations; require flip-condition |
| Framing mismatch | Judge compares unlike things | Match shot/format/length to reference |
| Fabricated verdict | Judge cites things not present | Require evidence quoted from the artifact |
| Sunk-cost looping | Loop 6 on an unreachable gap | Wrong-bar detection; re-scope |

---

## Reference files

- `references/judging.md` — the blind comparison protocol, judge prompt template, verdict format. Read before the first judge pass.
- `references/domains.md` — how the pattern adapts per domain: visual/game, software, research, writing, design. Read the relevant section at Phase 0.
- `references/prompt-template.md` — fill-in-the-blank prompt for handing this whole loop to an agent in one shot. Use when someone wants a paste-ready prompt rather than an interactive run.
