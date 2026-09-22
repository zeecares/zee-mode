# zee-mode

A git-installable plugin of engineering machinery for coding agents:
a steering vocabulary of principles, a verification-skill generator, an
adversarial review loop, and a judge-calibration loop. Plain markdown,
no runtime, no model lock-in. Works with any agent host that reads skills
(Claude Code, Cursor, or a plain directory of markdown an agent can load).

The shape comes from [pstack](https://github.com/cursor/plugins/tree/main/pstack)
(MIT, Lauren Tan): the machinery, without the host-specific glue. No model
panel, no routing config, no vendor wiring. What remains is the part that
was already portable: markdown that makes an agent rigorous.

## What's in it

- **principles/** - twelve engineering principles as a steering vocabulary.
  You steer an agent by naming a principle; the name points at a rule it has
  already read. See `principles/README.md`.
- **skills/create-verification-skill/** - interviews a repo and generates a
  project-local skill that drives the real app the way a user does, captures
  evidence, and proves one feature end to end before handover.
- **skills/calibrate/** - the calibration loop for LLM judges: dev/sealed
  dataset split, per-criterion confusion matrices, kappa / TPR / TNR / Brier /
  ECE, threshold sweeps, abstention coverage, disagreement triage, judge
  pinning, trust-or-escalate routing. This is the piece eval platforms don't
  give you: they store scores, they don't make the judge trustworthy.
- **skills/interrogate/** - adversarial review of a diff by several models at
  once. The signal comes from model diversity, not personas. Produces a
  synthesized verdict; never auto-applies changes.

## Install

Clone it where your agent loads skills or plugins from, or vendor the
pieces you want into a repo's own skills directory:

```bash
git clone https://github.com/zeecares/zee-mode.git
```

## Use

There is no mode command and no setup wizard. Point the agent at the piece
you need:

- "read the principles index, then apply prove it works to what you just built"
- "run create-verification-skill on this repo"
- "interrogate this diff"
- "run the calibrate loop on our judge, the dev set is labelled"

## Provenance and license

MIT, Ziyi Wang. Adapted from pstack (MIT, Lauren Tan) - see `NOTICE` for
exactly which parts are adapted and where the originals live.
`skills/calibrate/` is original.
