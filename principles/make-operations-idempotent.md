# Make Operations Idempotent

A retried operation must converge on the same end state as a single run.

**Why:** distributed work, flaky networks, and human impatience all guarantee
retries. If running twice differs from running once, the retry policy is a
bug farm.

**The pattern:**

- Writes keyed by stable identity: upsert, do not append.
- Before creating, check whether the end state already exists and return it.
- If a previous run can crash halfway, the next run reconciles what it finds
  instead of assuming a clean start.
- "It depends on what state was left behind" is the smell of a missing
  reconciliation step.
