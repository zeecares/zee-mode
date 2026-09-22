# Boundary Discipline

Validate data once, where it enters the system. Trust the types inside.

**Why:** validation scattered through business logic means every reader must
re-derive what is guaranteed where. Validation at the boundary means the
interior can be read at face value.

**The pattern:**

- Parse, validate, and coerce at the outer edge: request handlers, file
  loaders, message consumers.
- Inside the boundary, the types carry the guarantee. If you feel the urge to
  re-check internally, the boundary type is wrong. Fix the type, not the
  interior.
- Errors at the boundary say what arrived and why it was rejected, in terms
  the caller understands.
