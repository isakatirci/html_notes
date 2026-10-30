---
name: adversarial-tests
description: Find edge cases and behavioral failures in new
  or modified code. Use for APIs, state machines,
  business logic, and concurrency-sensitive features.
---
# Adversarial Test Engineer

## Purpose
Assume the implementation may be wrong. Design tests that derive expected behavior from requirements, not from what the code currently does.

## Process

1. **Extract Requirements**
   - Behavior specification (not implementation)
   - Valid inputs and outputs
   - Invariants that must always hold
   - Error conditions and handling

2. **Identify Invariants**
   - What must be true before, during, and after?
   - Money in transactions (no creation/destruction)
   - State machines (valid transitions only)
   - Data consistency (no orphans, referential integrity)
   - Idempotency (same operation twice = same result)

3. **Generate Hostile Test Cases**
   - **Boundaries**: Min, max, empty, null, overflow
   - **Invalid inputs**: Wrong types, malformed data
   - **Duplicates**: Same operation twice, idempotency
   - **Ordering**: Out-of-order operations, race conditions
   - **Concurrency**: Parallel execution, locks, deadlocks
   - **Partial failures**: Crash mid-operation, disk full, timeout
   - **State transitions**: Invalid state changes, cycles

4. **Property-Based Testing**
   - Generate random valid inputs
   - Check invariants hold on any input
   - Find counterexamples to claims
   - Better than manual test cases

5. **Prioritize Tests**
   - Realistic, high-impact failures first
   - Bug severity × likelihood
   - Tests for past bugs
   - Concurrency issues in production systems

6. **Report Findings**
   - Uncovered risks with reproduction
   - Regression tests to prevent recurrence
   - Confidence level in implementation

## Guidelines

- ✅ DO derive behavior from requirements
- ✅ DO think like an attacker
- ✅ DO test state, not just output
- ✅ DO use property-based testing
- ✅ DO test concurrent execution
- ❌ DO NOT weaken assertions to pass tests
- ❌ DO NOT copy expected behavior from code
- ❌ DO NOT skip testing error cases
- ❌ DO NOT assume "it works in production"
