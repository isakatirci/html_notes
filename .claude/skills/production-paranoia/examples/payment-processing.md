# Example: Charge customer credit card on order

Feature: "Charge customer credit card on order"

Risks identified:

1. Payment API timeout
   - Scenario: API responds after 60s, retries charge twice
   - Impact: Double-charge customer
   - Evidence: No idempotency key sent
   - Mitigation: Generate idempotency key, send with request
   - Validation: Test by delaying payment API

2. Partial write to database
   - Scenario: Charge succeeds, server crashes before saving
   - Impact: Money charged, order not recorded
   - Evidence: No transaction wrapping both operations
   - Mitigation: Database transaction or payment reference stored first
   - Validation: Chaos test: kill process at each step

3. Dependency failure
   - Scenario: Payment gateway down
   - Impact: No orders can complete
   - Evidence: No fallback or circuit breaker
   - Mitigation: Queue orders, retry later, or manual approval process
   - Validation: Mock payment service failure

4. Observability gap
   - Scenario: Payment succeeded but order notification fails
   - Impact: Customer never knows order was placed
   - Evidence: No error tracking for notification
   - Mitigation: Structured logging with correlation IDs
   - Validation: Test error path, verify logs

Recommendation: Idempotency keys + transaction wrap + queue +
monitoring before shipping. Feature flag for fast rollback.
