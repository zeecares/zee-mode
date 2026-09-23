# Keep Behavior, Drop Structure

When you replace or simplify a system, treat what it does today as the spec
and how it does it as negotiable.

**Why:** a system in use is a prototype that has already met real inputs.
Its observable behavior holds lessons nobody wrote down. Its internal shape
holds accidents: a layer added for a need that passed, the same fact stored
twice, one flag doing two jobs. A rewrite that copies the shape keeps the
accidents. A rewrite that ignores the behavior loses the lessons.

**The pattern:**

- Write down the behavior callers rely on before changing anything, and pin
  it with tests that call the system the way its users do.
- Look for things tied together that change for different reasons: a value
  and the time it was true, a rule and the code that enforces it, current
  state and the history that produced it, one status field standing in for
  several facts. Pull them apart.
- One fact, one owner. A cache, index, or copy is derived and can be rebuilt
  from its source; it never decides anything the source disagrees with.
- A replacement proposal names what it deletes. No deletion list, no
  simplification.
- Judge the whole system, not the diff. A change that adds a little in one
  place and removes more overall is simpler. A change that only moves
  complexity somewhere else is not.

Leave behind the same behavior with fewer moving parts, and the list of what
went away.
