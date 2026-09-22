# Fix Root Causes

Reproduce the defect and trace it to the cause before changing code.

**Why:** a fix aimed at a symptom papers over the real problem and adds a
workaround the next reader must explain. Guard clauses that mask invariant
violations, retry logic that hides a broken contract, casts that silence a
modeling error: all symptoms treated as causes.

**The pattern:**

- Reproduce first. No reproduction, no fix.
- Trace the execution path to where the wrong value is born, not where it is
  observed.
- If you see a workaround, ask what a proper fix looks like and why it is not
  in place.
- The fix belongs at the layer where the invariant broke, which is often a
  different module than the one that crashed.
