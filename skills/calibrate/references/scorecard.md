# Calibration report - <judge name> <version>

Date:
Author:
Judge model (pinned):
Rubric version (commit):
Prompt version (commit):
Dev set (id, size, labeler):
Sealed test set (id, size):

## Headline

| Metric | Dev | Sealed |
|---|---|---|
| Cohen's kappa | | |
| TPR | | |
| TNR | | |
| Brier (if probabilistic) | | |
| ECE (if probabilistic) | | |
| Abstention rate | | |
| Coverage at chosen threshold | | |

## Per-criterion confusion matrices

One 2x2 per criterion. Name the criterion, the threshold, and the cell
counts. Persistent off-diagonal mass between the same two outcomes is a
rubric definition problem - fix the rubric, not the labeler.

## Threshold sweep

Score threshold vs TPR/TNR/coverage. State the chosen threshold and the
margin that defines abstention.

## Failure clusters

Clusters of dev disagreements, each with a one-line attribution: rubric gap,
judge blind spot, or genuine human ambiguity. Genuine ambiguity stays in the
human queue - do not tune the judge to win ambiguous cases.

## Policy decisions

- Abstention routing:
- What counts as escalate-to-human:
- Recalibration cadence:

## Verdict

Accepted as the pinned judge for CI: yes / no. If no, what re-enters the
loop.
