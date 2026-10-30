---
name: root-cause
description: Investigate regressions, failing tests, and
  runtime errors when the underlying cause is unclear.
---
# Root Cause Detective

## Purpose
Never fix a symptom without understanding the disease. Before proposing a fix, produce competing explanations and find evidence that distinguishes them.

## Process

1. **Capture the Failure**
   - Exact error message and stack trace
   - Reproduction steps (minimal)
   - Frequency (consistent, flaky, one-time)
   - First observed and last known good version
   - Environment (dev, staging, production)

2. **Find Earliest Deviation**
   - When does the error first appear in the trace?
   - Is it in the code or in its dependencies?
   - Did recent changes affect this code path?
   - Are there timing or concurrency clues?

3. **Generate Competing Hypotheses**
   - At least 2-3 distinct explanations
   - Each should explain the observed behavior
   - Avoid "magic" or "I don't know" hypotheses

4. **Test Each Hypothesis**
   - **Supporting evidence**: What would confirm this?
   - **Contradicting evidence**: What would disprove it?
   - **Falsifiable test**: Design a test that could rule it out
   - Run only safe experiments first

5. **Report Findings**
   - Most likely root cause and confidence level
   - Alternative explanations still possible
   - Smallest fix that addresses root cause
   - Regression test to prevent recurrence
   - Why other "fixes" would be wrong

## Guidelines

- ✅ DO form multiple hypotheses
- ✅ DO design falsifiable tests
- ✅ DO separate measurements from guesses
- ✅ DO allow "I don't know yet"
- ❌ DO NOT hide failures with timeouts
- ❌ DO NOT swallow exceptions
- ❌ DO NOT claim untested hypotheses are proven
- ❌ DO NOT add sleeps or retries without investigation
