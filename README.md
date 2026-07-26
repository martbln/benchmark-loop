# benchmark-loop

A Claude Skill for reaching "actually as good as X" — not "looks good to me" — on ambitious, open-ended builds.

## The problem

"Make this AAA / world-class / production-grade / perfect" fails for a boring reason: nobody defined what that means, so whatever builds the thing also grades the thing. A builder inspecting its own work says "looks good" by default, every time. Fanning out into more sub-agents doesn't fix this — it just multiplies the same self-grading problem across more workers, and produces pieces that don't agree with each other in the process.

## The idea

Two changes fix most of it:

1. **Borrow the bar from something that already exists.** Not "AAA games" — a specific level from a specific title. Not "good writing" — a specific piece by a specific author. A named, sourceable reference turns a vague adjective into something you can hold two things next to.
2. **Judge blind.** A fresh reviewer, with no build history, shown both artifacts unlabeled and in random order, has no reason to flatter either one. It picks one or says it can't tell — and names the specific, evidence-backed thing that would change its mind. That becomes the next task.

Everything else in the skill is plumbing to make that comparison valid: picking a slice small enough to actually reach parity on, freezing shared foundations before parallelizing (so components don't drift apart), ranking which dimensions actually decide the verdict, and knowing when to stop.

## Install

**Claude Code** — personal (all projects):
```bash
mkdir -p ~/.claude/skills/benchmark-loop
cp -r SKILL.md references ~/.claude/skills/benchmark-loop/
```
Project-scoped instead: same layout under `.claude/skills/benchmark-loop/` in your repo.

Restart Claude Code, then confirm with `/skills`.

**claude.ai** — upload `benchmark-loop.skill` (built with `package_skill.py` from this folder) via the skill card's Save button, or Settings → Features → Add custom skill.

## Structure

```
benchmark-loop/
├── SKILL.md                       — core loop: Phase 0–4, stop conditions, failure modes
└── references/
    ├── judging.md                 — the blind comparison protocol + judge prompt
    ├── domains.md                 — adaptations per domain (games, software, research, writing, design, data)
    └── prompt-template.md         — fill-in-the-blank prompt for handing the whole loop to an agent in one shot
```

`SKILL.md` is what loads automatically when the skill triggers. The `references/` files are pulled in as needed — read `judging.md` before the first comparison, and the relevant section of `domains.md` when scoping the task.

## When it triggers

Written to fire on requests like:
- "at the level of [named thing]"
- "AAA" / "world-class" / "production-grade" / "publication-quality" / "not a prototype"
- "loop until it's perfect" / "fan out and don't stop until..."
- large, open-ended builds (games, apps, research reports, decks, design systems) where the hard part is holding a quality bar rather than knowing the steps

Skip it for small or purely functional tasks, or when speed matters more than polish.

## What it actually does

1. **Phase 0 — set the bar.** Name a specific reference, pick a comparison slice small enough to reach parity on, rank the dimensions that matter, write a checkable acceptance test.
2. **Phase 1 — foundation first.** Build and freeze whatever every component depends on (render pipeline, data schema, core thesis) *before* parallelizing — this is the step naive "fan out everything immediately" approaches skip, and it's why their parts don't compose.
3. **Phase 2 — fan out** on what's genuinely independent, against the frozen foundation.
4. **Phase 3 — the judge loop.** Produce a comparable artifact, judge it blind (fresh context, labels stripped, order randomized), get a verdict plus evidence plus a flip condition, fix exactly that, repeat.
5. **Phase 4 — integration pass** on the assembled whole, since components that individually pass can still fail together.
6. **Stop conditions**, set up front, so "don't stop until perfect" actually terminates: parity reached, iteration cap hit, diminishing returns, or an unreachable bar detected and re-scoped.

## Example

Original prompt this pattern was extracted from asked for "a first-person shooter at the level of the most recent Call of Duty... utterly perfect... fan out sub-agents... loop until perfect." See `references/prompt-template.md` for that request rewritten through the skill, plus a non-visual (research writing) example — because a purely aesthetic blind judge is actually dangerous in that domain: a confident fabricated claim reads *better* than an honest hedged one, so that example pairs blind comparison with a source-verification pass.

## Notes

- The judge must be genuinely isolated from the build — a fresh subagent, or a separate conversation with no shared history. Judging in the same context that built the work defeats the entire mechanism.
- "Harsh critic" is scoped, not unbounded — brutal on the top-ranked dimensions, silent on the rest until those close, and every criticism must cite something observable in the artifact. An unscoped critic just invents faults forever.
- This loop is expensive by design. It's for the subset of requests where the ambition is real and a reference exists — not a default for everyday tasks.

