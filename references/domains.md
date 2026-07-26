# Domain adaptations

Read the relevant section during Phase 0. Each gives: what the reference usually is, what the comparison slice should be, which dimensions tend to carry the verdict, what the foundation is, and where the loop typically goes wrong.

## Contents
- Visual / games / 3D
- Software & product UI
- Research & analysis
- Writing
- Design systems & brand
- Data work

---

## Visual / games / 3D

**Reference:** a specific level, scene, or shot from a named title. Screenshots and gameplay footage are readily available and are the comparison currency.

**Slice:** one scene, one environment, one 30–60 second interaction. Never the whole game.

**Dimensions that usually decide it:** lighting and shadow behavior first (contact shadows, bounce, falloff), then material response (roughness, specular, how surfaces read at grazing angles), then animation weight and feel. Geometry density and texture resolution matter far less than people assume — a low-poly scene with correct lighting reads as more professional than a dense scene with flat lighting.

**Foundation (build serially):** render pipeline, lighting model, tone mapping, color space, material standard, unit scale. Every component inherits these. Parallelizing before they're frozen produces assets that can't sit in the same frame.

**Where it goes wrong:** judging on beauty shots instead of representative gameplay frames; chasing polygon counts; comparing a static render to a real-time frame; targeting a bar that requires motion capture or a decade of engine work — detect that early and re-scope rather than looping against it.

---

## Software & product UI

**Reference:** a specific app's specific flow. "Linear's issue creation," "Stripe's checkout," "Things' quick entry."

**Slice:** one flow end to end, including the empty state, the error state, and the loading state — those three are where amateur builds reveal themselves and where references are quietly excellent.

**Dimensions:** interaction feel (latency, transitions, focus behavior, keyboard support), then information density and hierarchy, then visual finish. Correctness is a precondition, not a dimension — broken doesn't get judged.

**Foundation:** design tokens, component primitives, state model, routing.

**Judging:** compare recorded interactions, not static screenshots. Half the quality of a good interface is in what happens between frames. Also judge one flow blind on *speed to complete* — a real usability signal that survives blinding.

**Where it goes wrong:** comparing a static mockup to a live product; skipping edge states; polishing the happy path only.

---

## Research & analysis

**Reference:** a specific published piece in the target genre — a named review article, a Stratechery or Matt Levine post, a specific equity research note, a specific policy brief.

**Slice:** one section — the literature synthesis, or the central argument, or the methods section. Not the whole report.

**Dimensions:** claim-to-evidence integrity first (does every non-obvious assertion trace to a real, checkable source), then reasoning quality (are alternatives considered and ruled out, or only the favored answer supported), then structure, then prose.

**The judge must verify, not just compare.** This is the domain where a purely aesthetic judge is actively dangerous: fabricated citations and confident unsupported claims read *better* than honest hedged ones. So add a verification pass — sample claims at random, check each against its cited source, and fail the round on any that don't hold. A polished report built on invented sources is worse than a rough one.

**Foundation:** the question, the definitions, the scope boundaries, the evidence standard. Fan out sections before these are fixed and you get sections that contradict each other's terms.

**Where it goes wrong:** matching the reference's confident tone without its evidence base; optimizing readability over accuracy; loops that improve prose while the argument stays weak.

---

## Writing

**Reference:** a specific piece by a specific author. Genre-matched — you cannot judge a technical explainer against a personal essay.

**Slice:** 500–1000 words of the same section type. Openings against openings.

**Dimensions:** whether it earns attention paragraph by paragraph, then specificity (concrete detail vs. abstraction), then sentence rhythm. Note that "matching the reference's voice" is usually the wrong target — matching its *quality level* in your own voice is the real one. Say which you're doing.

**Judging:** blind comparison works unusually well here. Also useful: give the judge only the first 150 words of each and ask which it would keep reading.

**Where it goes wrong:** imitating surface tics of the reference author; polishing sentences while structure stays broken.

---

## Design systems & brand

**Reference:** a named system — Material, Stripe, Linear, a specific brand book.

**Slice:** one component family across all its states, or one page composed from the system.

**Dimensions:** internal consistency (does the same idea look the same everywhere), then typographic scale and spacing rhythm, then color system behavior under real content.

**Foundation:** the scale itself — type ramp, spacing units, color ramps, radius scale. Almost everything here is foundation, so this domain parallelizes less than others.

**Where it goes wrong:** judging components in isolation, where inconsistency is invisible. Judge them assembled on a real page with real content lengths.

---

## Data work

**Reference:** a published dataset or analysis with documented methodology.

**Slice:** one pipeline stage, or one chart, or one table.

**Dimensions:** correctness and reproducibility first — these are checkable, so check them programmatically rather than by judgment. Then presentation.

**Judging:** blind comparison applies to the *presentation* layer (charts, tables, framing). For the analysis layer, prefer assertions over judges: a script that verifies row counts, null handling, and joins beats any critic's opinion. Use the loop where taste matters and tests where truth matters.
