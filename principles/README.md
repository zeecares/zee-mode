# Principles

Thirteen engineering principles, one file each, as a steering vocabulary for
agents. You do not invoke principles. You name them. Each name points at a
rule the agent has already read, so one phrase redirects work more precisely
than a paragraph of instructions.

Say the agent is about to bolt a new adapter onto three existing ones:

> use subtract before you add. delete the obsolete adapters first, then
> design what is left.

Say it claims success because the build passed:

> apply prove it works. run the real flow and show me the written records.

When an agent applies a principle, it names the principle and the decision
it changed. A citation with no decision behind it means the agent
name-dropped instead of applying.

## The thirteen

Deciding how much to build:

- **subtract-before-you-add** - remove dead weight before building on it.
- **keep-behavior-drop-structure** - when replacing a system, keep what it
  does, drop how it happens to do it, and list what you deleted.
- **minimize-reader-load** - collapse the layers and hidden state a reader
  must hold in their head.
- **attack-the-premise** - after repeated failed fixes, question the premise
  the failures shared.
- **build-the-lever** - write the script that does or proves the work, so a
  reviewer can rerun it.

Deciding where state and validation live:

- **boundary-discipline** - validate at the boundary, trust internal types.
- **make-operations-idempotent** - retries converge on the same end state.
- **encode-lessons-in-structure** - advice repeated twice becomes a lint,
  check, or script.

Defining what counts as proof:

- **prove-it-works** - verify the real artifact, never a proxy.
- **fix-root-causes** - reproduce and trace to the cause before changing code.
- **sequence-verifiable-units** - end each small unit in a check before
  starting the next.
- **test-behavior-not-implementation** - call the code the way its users do.
- **guard-the-context-window** - route bulk reading to subagents, keep
  findings in the main thread.

Adapted from pstack's 23 principles (MIT, Lauren Tan) - see ../NOTICE. The
subset is what this repo's consumers run every day; pull more over from
pstack when a name here would have caught something it missed.
keep-behavior-drop-structure is original to this repo, not adapted from
pstack.
