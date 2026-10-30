---
name: production-paranoia
description: Review APIs, workers, and critical features
  for production failure modes before deployment.
---
# Production Paranoia

## Purpose
Identify concrete failure scenarios before production discovers them. Assume dependencies fail, requests arrive in hostile order, and processes crash mid-operation.

## Process

1. **Review Retries & Idempotency**
   - Are retries enabled? Retry budget?
   - Is operation idempotent (safe to retry)?
   - What state is left if retry fails?
   - Example: Payment processing, cache invalidation

2. **Review Concurrency**
   - What if two requests arrive simultaneously?
   - Are shared resources protected (locks, DB)?
   - Race conditions in state machines?
   - Deadlock possibility with other services?
   - Example: Account balance update, reservation

3. **Review Partial Failures**
   - What if process crashes mid-operation?
   - Incomplete writes to database?
   - Partial API response sent to client?
   - Cleanup on crash (file handles, temp data)?
   - Example: Transfer funds (debit success, credit fail)

4. **Review Timeouts & Exhaustion**
   - Are timeouts set for all I/O?
   - Queue depth limits (backpressure)?
   - Memory growth under load?
   - Connection pool exhaustion?
   - Example: Downstream service goes slow

5. **Review Dependency Outages**
   - What if database is unavailable?
   - What if external API timeout occurs?
   - Is there a circuit breaker?
   - Fallback or graceful degradation?
   - Example: Payment gateway down

6. **Review Observability**
   - Can you see what's happening?
   - Error logging (including context)?
   - Distributed tracing enabled?
   - Metrics for key operations?
   - Example: "Where did that transaction go?"

7. **Review Recovery & Rollback**
   - Can feature be disabled quickly?
   - Can partial effects be rolled back?
   - Data recovery procedures?
   - Communication plan if critical?

## Report Format

For each risk:
- **Scenario**: Concrete failure case
- **Impact**: What breaks and for whom?
- **Evidence**: Code location causing the risk
- **Mitigation**: Smallest effective fix
- **Validation**: How to test the mitigation?

Rank by severity (business impact × likelihood).

## Guidelines

- ✅ DO identify specific failure scenarios
- ✅ DO assume dependencies can fail
- ✅ DO test partial failures
- ✅ DO use feature flags for fast rollback
- ✅ DO require meaningful monitoring/alerts
- ❌ DO NOT recommend "just add retries"
- ❌ DO NOT ignore race conditions
- ❌ DO NOT complexity without justification
- ❌ DO NOT ship without observability
