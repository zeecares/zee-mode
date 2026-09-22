---
name: interrogate
description: "Adversarial multi-model review of a change. Use for \"interrogate\", \"adversarial review\", \"challenge this\", \"stress test this code\", or \"find blind spots\". Several independent reviewers tear at the same diff; you synthesize a verdict. Never auto-applies changes."
---

# Interrogate

Send the same diff to several reviewers, one per model you can field, and
synthesize their findings into one verdict. The adversarial signal comes from
model diversity, not assigned personas: different model families have
different blind spots, so the same prompt to three models is three different
reviews.

The deliverable is a synthesized verdict. Do NOT auto-apply changes.

## Step 1 - Determine scope

- If the user points at files or a diff, use that.
- If on a feature branch, diff against the base branch for the full
  changeset.
- If the message references recent work, gather the relevant files.

Package the diff plus the surrounding context a reviewer needs to understand
the code.

## Step 2 - State the intent

Write one clear paragraph stating what the change is supposed to do, derived
from the user's message, commit messages, the PR description, or the code
itself. If the intent is unclear, ask before proceeding. Reviewers challenge
execution, never the intent.

## Step 3 - Spawn reviewers

One reviewer per available model. How you field them depends on the host:

- A host with subagents: spawn one per model, in parallel, read-only.
- A host with one model: run the same prompt yourself, then ask the user to
  paste the prompt into one or two other models and bring the outputs back.
  A two-model interrogate run by hand beats a one-model review.

Every reviewer gets the identical filled template from
`references/reviewer-prompt.md`: the stated intent, the diff, the rubric from
`references/rubric.md`. Identical input is what makes disagreement
meaningful.

## Step 4 - Synthesize

Collect findings and merge duplicates across reviewers. For each surviving
finding, classify:

- **Confirmed:** more than one reviewer found it independently, or one found
  it and you can verify it by reading the code. Verify before confirming -
  reviewers hallucinate too.
- **Contested:** exactly one reviewer found it and you cannot confirm. Report
  it as contested with the reviewer's evidence, not as fact.
- **Rejected:** verifiably wrong. Note the rejection in one line so the user
  knows it was considered.

Order the verdict by severity: critical, warning, nit. State what was NOT
found as plainly as what was - "no correctness issues found by any reviewer"
is information.

## Step 5 - Report, then stop

Present the synthesized verdict. The user decides what gets fixed. Applying
findings without their sign-off defeats the point of an adversarial pass.
