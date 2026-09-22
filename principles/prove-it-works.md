# Prove It Works

Verify every task output against the real artifact. Never a proxy, a
self-report, or "it compiles".

**Why:** unverified work has unknown correctness. Indirect verification -
file timestamps, output freshness, a delegate's summary - feels cheaper than
direct observation. Acting on a wrong inference costs far more than checking
the source.

**The pattern:**

- After completing any task, ask: how do I prove this actually works?
- Check process liveness directly, not through derived state.
- Read the actual value, not a cached representation.
- For a feature: build it, run it, exercise the real user path, and check
  that data flows from input to output. Include the side effects: files
  written, rows inserted, messages sent.
- For delegated work: inspect the artifact - the diff, the file contents, the
  runtime behavior - not the delegate's summary.
- When verification fails, suspect the observation method before suspecting
  the system.

The strongest proof is a deterministic script that reruns the same
comparison. Write it, run it, keep the output as the artifact (see
build-the-lever).
