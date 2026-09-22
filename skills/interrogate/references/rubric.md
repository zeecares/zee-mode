# Review Rubric

Review through whichever lenses are relevant. Not every lens applies to every
change. Use judgment.

## Correctness

Does the code do what the intent says it should?

- Edge cases: empty inputs, nil/undefined, boundary values, concurrent access.
- Error handling: caught, propagated, or silently swallowed?
- Off-by-one, type coercion, overflow, encoding.
- Idempotency: what happens if this runs twice, or a previous run crashed
  halfway? "It depends on what state was left behind" means a missing
  reconciliation step.
- Concurrency: shared mutable state protected structurally, or by conventions
  that will not hold?

When you find a potential bug, trace the execution path. Do not flag "this
could be nil" - show the call chain that makes it nil.

## Root causes vs symptoms

Is the change fixing the problem or papering over it?

Look beyond the changed files. Read callers, callees, types, sibling modules.
Understand why the code exists before judging whether the change addresses
the right layer.

- Guard clauses that mask a deeper invariant violation.
- Retry logic that hides a broken contract.
- Casts that silence a modeling error.
- Instructions where structure would be better: a comment saying "don't do X"
  or a convention someone must remember, where a type constraint, lint rule,
  or runtime check could make the wrong thing impossible.

## Structural integrity

Does the change fit the system it lives in?

- Boundary discipline: validation at the edges, or scattered through business
  logic?
- Abstraction level: high-level orchestration mixed with low-level detail?
- Data model fit: do the structures match the access patterns?
- Bolted-on vs integrated: if the requirement had been known from day one,
  would the code look like this?
- Legacy dual-paths: a new API introduced while the old one stays alive, with
  no external consumers? Migrate callers and delete the old path in the same
  wave.

Do not penalize simple code for lacking abstraction. Premature abstraction is
worse than duplication.

## Verification

Can you tell the code works from reading it?

- Are there tests? Do they test behavior or implementation details?
- If this is a bug fix, is there a test that fails on the old code?
- Does anything verify the real artifact, or only proxies and self-reports?

## Complexity budget

Is the complexity justified by what the code accomplishes?

- Abstractions serving one call site. Configuration for cases that do not
  exist. Dead code, unused imports, vestigial parameters.
- Obsolete compatibility scaffolding kept alive after the migration ended.
- Half-finished features are worse than missing ones.

Simpler is better unless simpler is wrong.

## Security

Flag only issues you can trace through the code. "This could be an injection
vector" without the input path is not useful.

- User input flowing to dangerous sinks (SQL, shell, eval, HTML) unsanitized.
- Auth gaps in new endpoints. Secrets in code, logs, or error messages.
