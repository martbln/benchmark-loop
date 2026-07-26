# Paste-ready prompt template

For handing the whole loop to an agent in one shot instead of running it interactively. Fill the brackets. Delete lines that don't apply — a template with unfilled placeholders performs worse than a shorter honest one.

---

## Template

```
Build [DELIVERABLE] at the quality level of [SPECIFIC NAMED REFERENCE].

THE BAR
The reference is [NAMED ARTIFACT — specific title, post, product, level, paper].
We are matching one slice, not the whole thing: [SLICE — e.g. "one 60-second
indoor firefight", "the issue-creation flow including empty and error states",
"the literature synthesis section"].

Quality here means, in priority order:
1. [PRIMARY DIMENSION — the one that carries the verdict]
2. [SECONDARY DIMENSION]
3. [everything else — explicitly deprioritized until 1 and 2 close]

ACCEPTANCE TEST
A judge shown our output and the reference, unlabeled and in random order,
picks ours or says it can't tell — on at least 2 of 3 runs.

FOUNDATION FIRST
Before parallelizing anything, build and freeze the shared substrate:
[FOUNDATION — render pipeline and lighting model / data schema / design
tokens / thesis and definitions]. Write it down. Everything downstream
references it. Do not fan out until this is fixed.

THEN FAN OUT
Decompose the slice into components that depend on the foundation but not on
each other. Work them in parallel. Give each worker the frozen foundation
spec, its scope, what it must not touch, and the reference for its component
specifically.

THE JUDGE LOOP
For each component:
1. Produce a comparable artifact — same framing, format, and length as the
   reference. Strip anything identifying which is which.
2. Hand both to a fresh judge with no build history, in random order.
3. Require: verdict (A / B / indistinguishable), three specific observations
   citing things actually visible in the artifact, and the single change that
   would most flip the verdict.
4. Fix exactly the named gaps. Re-judge with fresh eyes and new ordering.

Be harsh on dimensions 1 and 2 and ignore the rest until they close. Every
criticism must point at something concrete — no "needs more polish."

STOP WHEN
- Parity reached per the acceptance test, OR
- [N] iterations on a component (pick 4-6), OR
- Two consecutive rounds where complaints shrink and drift to deprioritized
  dimensions — that's the ceiling of this approach, and more looping polishes
  the wrong thing, OR
- The judge keeps citing something structurally unreachable [mocap, proprietary
  data, years of engine work]. Say so plainly instead of looping against it.

FINALLY
Run one judge pass on the assembled whole — seams and tonal inconsistency only
show up there.

Report honestly at the end: which components reached parity, which didn't, and
what the residual gap is. Don't tell me it's done if it isn't.
```

---

## Worked example 1 — the original request, rewritten

The prompt this pattern came from asked for "a first-person shooter at the level of the most recent Call of Duty, utterly perfect, AAA everything, fan out sub-agents, loop until perfect." Rewritten:

```
Build a first-person shooter slice in ThreeJS at the visual quality of Modern
Warfare II.

THE BAR
Reference: MWII's indoor combat environments — specifically the lighting and
material response in its interior levels.
Slice: one 60-second indoor firefight in a single room. Not a campaign, not
multiplayer, not a menu system.

Quality here means, in priority order:
1. Lighting behavior — contact shadows, bounce, falloff, how light dies in
   corners
2. Material response — roughness, specular at grazing angles, how metal and
   painted surfaces differ
3. Animation weight, audio, geometry density — deprioritized until 1 and 2 close

ACCEPTANCE TEST
A judge shown a representative gameplay frame from our build and a real MWII
interior frame, unlabeled and randomly ordered, picks ours or can't tell, on
2 of 3 runs. Representative frame, not a beauty shot from the one good angle.

FOUNDATION FIRST
Freeze before parallelizing: render pipeline, tone mapping, color space,
lighting model, material standard, unit scale. Assets built against different
assumptions here cannot share a frame.

THEN FAN OUT
Components: environment geometry, materials, lighting setup, weapon model and
handling, enemy behavior, audio. Each gets the frozen spec and its own
reference frames.

[judge loop, stop conditions, integration pass as in the template]
```

The changes that matter: a named level instead of "recent Call of Duty," one room instead of a game, a ranked dimension list so the loop doesn't spend four rounds on muzzle-flash particles while the lighting reads flat, foundation frozen before fan-out so the six components compose, and stop conditions so "don't stop until perfect" terminates.

---

## Worked example 2 — non-software

```
Write a market analysis of [SECTOR] at the level of a Stratechery deep-dive.

THE BAR
Reference: [specific named post].
Slice: the central argument section — roughly 1200 words. Not a full report.

Quality means:
1. Claim-to-evidence integrity — every non-obvious assertion traces to a real,
   checkable source
2. Reasoning — alternatives considered and ruled out, not just the favored
   answer supported
3. Prose and structure — after 1 and 2

ACCEPTANCE TEST
Two parts, both required:
(a) Blind comparison: judge picks ours or can't tell, 2 of 3 runs.
(b) Verification pass: sample 8 claims at random, check each against its cited
    source. Any that don't hold fails the round outright.

FOUNDATION FIRST
Fix the question, the definitions, the scope boundary, and the evidence
standard before drafting any section.

[loop and stop conditions as in the template]
```

Note part (b). In research the aesthetic judge is not enough and is actively dangerous on its own — a confident fabricated citation reads *better* than an honest hedged claim. Comparison judges style; verification judges truth. Run both.
