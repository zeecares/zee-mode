---
name: calibrate
description: "Align an LLM or typed-decision judge with human judgment and keep it aligned. Use when standing up an LLM-as-judge eval, iterating a rubric, adding a judge to CI, or when judge scores are trusted or distrusted without evidence."
---

# Calibrate

Eval platforms store scores. They do not make a judge trustworthy. Trust
comes from a calibration loop: a labelled dev set, a sealed test set,
agreement metrics against human judgment, and a discipline that every judge
or rubric change re-enters the loop. This skill is that loop.

Human attention goes to exactly three places: building the golden set,
labelling the calibration dev set, and reviewing disagreement. Everything
else is waste.

## Artifacts live in git

The platform holds traces and scores. Git holds the methodology:

- `datasets/` - the registry: which traces are dev, which are sealed test,
  and the labels. The split is enforced here, before any judge iteration.
- `rubrics/` - one rubric per subjective property, versioned.
- `judges/` - judge prompts and model pins, versioned.
- `reports/` - one calibration report per judge version
  (`references/scorecard.md` is the shape).

Pull requests on these directories are the audit trail. A judge change
without a report is uncalibrated by definition.

## The loop

1. **Split dev / sealed test first.** Before any judge iteration, freeze a
   sealed test set nobody tunes against. All iteration happens on dev. The
   sealed set runs once per accepted judge version, to confirm the dev
   numbers travel.
2. **One judge, one property.** Each judge answers one subjective question
   (faithfulness, presentation, would-an-analyst-trust-this). Everything that
   can be checked deterministically stays in code - judges earn their keep
   only on subjective properties. Binary pass/fail per judge, reasoning
   before verdict when the judge can reason.
3. **Label the dev set blind.** One domain expert labels 100-200 traces
   without seeing judge output. Blind labelling is what makes the comparison
   honest: it is inter-rater reliability between the expert and the judge.
4. **Measure agreement, not accuracy theater.** Per judge version, per
   criterion: confusion matrix, Cohen's kappa, TPR and TNR - never raw
   agreement, because an always-pass judge scores well on a mostly-good set.
   For probabilistic judges add Brier and ECE. Run a threshold sweep and
   record abstention coverage. Target kappa around 0.7 on dev.
5. **Iterate the rubric, not the labeler.** Persistent confusion between two
   outcomes is a rubric definition problem, not a labeler problem. Sharpen
   rubric language on dev until the confusion moves.
6. **Pin everything.** A calibrated judge is a model version plus a rubric
   version plus a prompt version. Pin all three. Recalibrate on any change to
   any of them, and on a fixed cadence regardless - judges drift under
   provider updates.
7. **Route disagreement to humans.** Low-confidence scores and cases where
   criteria disagree go to a human review queue. Trust-or-escalate: the
   judge's job is to absorb the easy mass, not to win every case.
8. **Gate CI on the pinned judge.** Regression gates run the pinned judge,
   never latest. A judge bump is a PR with a calibration report attached.

## Failure attribution comes before tuning

Scores do not tell you what to tune. When the loop shows disagreement,
cluster the failing traces and attribute the failures first; fix the rubric
or the system second. A kappa dip with unclustered failures is a queue, not
a verdict.

## Optimizers live inside the same discipline

Automated prompt optimization (GEPA-style reflection, or DSPy more generally)
is welcome for the judge prompt, under the same rules as manual iteration:

- Training data lives strictly inside dev. Never the sealed test.
- The metric is agreement with the human labels (kappa), not a proxy.
- An optimized judge faces the same acceptance bar as a hand-iterated one:
  full report, pinned versions, one sealed-test confirmation.

## Typed-decision judges (Jev and similar)

Some judges return typed decisions only - a probability, a score, a choice
distribution - with no reasoning. Work with that, do not decorate it:

- Aim for **diagnosable, not explainable**. Split one opaque score into
  atomic criteria so you can see which criterion moved and how uncertain it
  is. Never have a second LLM write a post-hoc rationale and present it as
  the judge's reason.
- **Ask for one distribution, not N independent questions.** Independent
  per-outcome probabilities do not normalize into a distribution. Asking
  drop / summarize / keep as three separate questions and then normalizing
  compresses confident answers (0.94 vs 0.85 becomes 0.51 vs 0.46) and
  manufactures abstentions that fail closed. Prefer a single
  mutually-exclusive choice question. If the API only offers independent
  scores, set policy on the raw scores - compare them yourself with explicit
  thresholds and margins - rather than normalizing.
- **Abstention is a routing signal, not a failure.** Low confidence routes to
  the human queue (step 7). A policy that treats abstention as "worst case"
  silently doubles human load; a policy that treats it as "best case"
  silently drops review. Pick one and record it in the report.

## Pairwise only for version-vs-version

Humans rank A against B far more consistently than they score on an absolute
scale. Use pairwise comparison only to compare two judge versions, with both
orderings shown to control position bias. Never use pairwise as the labeling
scheme for the golden set.

## Done means

A judge version with: a pinned model, rubric, and prompt; a dev report
hitting the kappa target with TPR/TNR, threshold sweep, and abstention
coverage recorded; one sealed-test run confirming the numbers travel; and a
CI gate running the pinned judge. Anything less is an experiment.
