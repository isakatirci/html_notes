# Example: `transferMoney(from, to, amount)`

Function: `transferMoney(from, to, amount)`

Requirements: Transfer money safely, no loss or gain, handle errors

Adversarial tests:
1. Transfer $0 (boundary)
2. Negative amount (invalid)
3. Sender = Receiver (edge case)
4. Transfer twice simultaneously (race condition, idempotency)
5. Crash after debit but before credit (partial failure)
6. Concurrent transfers from same account (state)
7. Overdraft attempt (validation)

Result: Found race condition where simultaneous transfers
can temporarily break invariant (total money != original).
Fix: Use database transactions with proper isolation level.
