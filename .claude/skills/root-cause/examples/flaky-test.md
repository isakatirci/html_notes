# Example: Test passes locally, fails randomly in CI

Failure: Test passes locally, fails randomly in CI (flaky)

Hypothesis A: Race condition in test setup
- Evidence: Timing dependent, CI hardware different
- Test: Run with thread sanitizer, add deterministic delays

Hypothesis B: Test suite pollution
- Evidence: Passes in isolation, fails in suite
- Test: Run tests in different orders

Hypothesis C: CI dependency timeout
- Evidence: Network calls sometimes slow
- Test: Mock slow endpoints, check timing

Result: Hypothesis A confirmed. Fix: Add proper synchronization
with barrier instead of sleep. Regression test: Thread sanitizer.
