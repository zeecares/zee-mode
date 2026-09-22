# Minimize Reader Load

Every layer of indirection, every piece of hidden state, is weight the next
reader must hold in their head. Collapse it.

**Why:** most code is read far more often than it is run, and agents reading
your repo pay the same tax humans do. A reader who cannot hold the system in
their head makes local edits with global side effects.

**The pattern:**

- Fewer layers, shallower call stacks, state that lives in one obvious place.
- Name things after what they do at the call site, not how they work inside.
- If understanding a function requires reading three other files, restructure
  until it does not.
- Prefer a boring linear flow over a clever dispatch table.
