# Test Behavior, Not Implementation

Call the code the way its users do. Assert on observable outcomes.

**Why:** implementation-coupled tests pass while behavior rots, and they
punish refactors. A test that mocks every collaborator verifies the mocks.

**The pattern:**

- Drive the public interface: the exported function, the CLI, the HTTP route.
- Assert literal expected values and observable side effects, not internal
  call counts.
- The deletion test: if the test would still pass when every imported
  function returned undefined, delete the test.
- A bug fix lands with a test that fails on the old code and passes on the
  new.
