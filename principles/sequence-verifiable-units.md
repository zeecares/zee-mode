# Sequence Work into Verifiable Units

Break work into small units that each end in a check. Start the next unit
only after the current one passes.

**Why:** a long chain of unverified steps fails at an unknown position, and
debugging ten steps at once costs more than checking ten times.

**The pattern:**

- Each unit is small enough that its check is obvious.
- The check is against the real artifact, not a proxy (see prove-it-works).
- If a unit cannot be verified on its own, the decomposition is wrong. Split
  differently.
- Report which units passed, so the reviewer can spot-check one instead of
  re-running all of them.
