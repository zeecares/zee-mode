# Guard the Context Window

The main thread's context is the scarcest resource in an agent run. Spend it
on decisions, not bulk.

**Why:** once the window fills with raw material, the reasoning that matters
gets squeezed out or truncated. Ten thousand lines of log in context is not
thoroughness; it is self-sabotage.

**The pattern:**

- Route bulk reading - logs, dumps, long files, search sweeps - to subagents
  or scripts. Bring back findings, not raw material.
- Keep the main thread for intent, judgment, and the artifacts under active
  edit.
- Before pasting anything long, ask whether a filtered projection answers the
  question.
