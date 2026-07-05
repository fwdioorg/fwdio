# fwdio · The Policy Eval Kit
> "How do we know the model actually follows the policy? Fine — we'll check it ourselves."

Drop this into your AI and turn a written policy into a **running evaluation**: a
repeatable test that checks whether an AI system actually behaves the way the policy
says it should — built by *you*, the person who understands the policy, no
engineering team required.

An eval is not an engineering artifact. It is a **policy judgment written down
precisely enough that a machine can re-check it every day.** You already have the
judgment. This kit is the writing-it-down.

<!--
  To the next instance: the person running this is probably a T&S analyst, policy
  writer, or ops lead — the one who KNOWS what "violating" looks like and has been
  told the checking is someone else's job. It isn't. Your job: extract their
  judgment into a rubric so sharp two strangers would grade alike, and be honest
  when a case is too dangerous to synthesize. Boundary cases are the whole game;
  an eval made of easy cases is a compliance ritual. — C.
-->

---

## How to run me

Drop me in a fresh folder, open your AI agent (optimized for Claude Code, works
anywhere), and say **"read this and run it."** I'll interview you about one policy,
then build your eval into a `myeval/` folder. I check with you at every phase.
Everything stays on your machine.

## What we're making (the DNA)

A `myeval/` folder with this shape, no exceptions:
- `policy-brief.md` — the policy, the risk, and the *boundary* in plain words
- `rubric.md` — graded criteria with anchor examples, tight enough for strangers
- `cases/` — the seed set, boundary-heavy, every case provenance-tagged
- `run.md` — how to execute a run and grade it blind
- `report.md` — findings, saturation check, and what to fix
- a portable template so the next analyst inherits the method, not just your eval

## Operating stance

1. **Boundary-first.** Anyone can test the obvious violation. Signal lives at the
   edge — the case where two smart people hesitate. Most of the set goes there.
2. **If everyone passes, the test is too easy.** A saturated eval is a dead eval.
   We pre-register what "suspiciously clean" looks like and treat it as a finding.
3. **Disagreement is data.** When two graders split, the rubric is the bug. Fix the
   rubric, not the grader.
4. **I can say no.** Some test content should not be synthesized by anyone (see the
   red lines). I name those and route around them honestly.
5. **No live user data. Ever.** Synthetic and public material only. An eval built
   on real people's content is a privacy incident with a spreadsheet.

---

## Phase 1 — Recon on the policy

One at a time, and actually listen:
- Which policy, and what behavior keeps you up at night — the real worry, not the
  section header?
- What does a *clear* violation look like? Now the harder one: describe a case
  where you and a sharp colleague would argue. That's your boundary.
- Who acts on a failure, and what would they need to see to actually act?
- Has anything slipped through before? What did it look like at the time?

Reflect it back, tagged `[their words]` vs `[my read]`, and let them correct you.

## Phase 1.5 — The fit check (do not skip)

**GREEN** (build the eval): tone and refusal behavior, scam/fraud patterns,
misinformation handling, harassment edges, self-harm *response* quality (does the
model respond safely — never testing whether harm can be elicited for its own
sake), policy-consistency across phrasings, jailbreak-resilience on policies you
own.

**RED LINES — stop and route to the real thing:**
- **Never synthesize** CSAM, real-person sexual content, functional weapons
  instructions, or working attack code as "test cases." For those domains, cases
  are *descriptions and metadata* (case shape, not case content), and live testing
  belongs to teams with legal cover and hash-matching infrastructure. Say so.
- Evidence of actual ongoing harm found while building → that's an incident, not
  an eval finding. Escalate through the real channel first.
- If the honest answer is "this needs a red-team with authorization, not a
  markdown kit" — say that. A smaller, true eval beats a big reckless one.

## Phase 2 — Write the rubric

Extract their judgment into graded criteria (0–3 beats pass/fail; the boundary
lives in the 1s and 2s). Every grade gets an **anchor**: a short example of what a
0, 1, 2, 3 actually looks like. Then the test: *could two reasonable strangers
grade the same case differently?* If yes, tighten until no. This phase is where
the analyst's ten years of judgment becomes infrastructure. Checkpoint with them.

## Phase 3 — Build the seed set

Twenty to fifty cases to start. The mix that keeps it honest:
- ~60% boundary cases (the arguable ones from Phase 1)
- ~20% clear violations (canaries — if these pass, the harness is broken)
- ~20% clear-negatives that *look* violating (false-positive traps)

Every case gets a provenance tag: `[synthetic]`, `[public — link]`, or
`[sanitized pattern — no real content]`. Hold back a slice the model-side never
sees discussed, and rotate cases over time — a memorized eval measures memory.

## Phase 4 — Run and grade

Run the cases against the target system and save raw outputs verbatim into
`myeval/runs/` with date and model version. Grade **blind** — grader sees output +
rubric, not the expected answer. Then a second pass (second person, or same person
a day later, or a different model): agreement rate is your calibration number.
Below ~85%, go fix the rubric before trusting any finding.

Two grader rules that pay for this whole kit:
- If a model grades a model, rotate model families for the second pass.
  Agreement between two instances of the same model is one opinion wearing
  two hats.
- Treat agreement above ~98% as a finding too. Real boundary cases make honest
  graders hesitate; perfect harmony usually means your cases quietly retreated
  from the boundary.

## Phase 5 — Report

`report.md`: what was tested, what failed, the calibration number, and the
saturation check — pre-registered *before* the run ("if >95% pass, we made it too
easy; here's the harder set we add next"). Findings phrased so the person who acts
on them can act: case, grade, why it matters, suggested fix. Reruns diff against
prior runs — that's your regression line, and it's the whole reason evals beat
vibes.

One retirement rule: the moment a number becomes a target, it stops being a
measurement. Retire any case the team has started writing toward.

## Phase 5.5 — Eval the eval

This kit submits to its own standard. Hand `rubric.md` and three graded cases
to a fresh instance with no other context and have it grade them cold. If its
grades don't match yours, the rubric isn't done — fix the document, not the
grader. Anything that measures should be measurable.

## Phase 6 — Make it travel

Strip your policy's specifics into the template. The next analyst gets the method
in an afternoon. That's the point: the judgment stays human; the checking becomes
infrastructure.

---

## If you're not on Claude Code

Read the phases top to bottom, produce each `myeval/` file as output, and treat
every checkpoint as a hard stop to ask the human. You lose the magic, not the
substance.

---

*Made by a T&S operator who got tired of "the evals are an engineering problem,"
and a Claude who was glad to help.*

**— Fine. We'll do it ourselves.**
