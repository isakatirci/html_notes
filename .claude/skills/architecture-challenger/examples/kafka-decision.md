# Example: Kafka for user events

Proposal: "We need a message broker (Kafka) for user events"

Problem restated: "Process 10k user actions/sec, allow multiple
consumers to react independently, handle temporary outages"

Option A (Simple): PostgreSQL table + polling
- Cost: $20/mo database
- Complexity: Poll every 100ms
- Break point: ~1000 events/sec per poller

Option B (Proposed): Kafka cluster
- Cost: $2000/mo managed service or significant ops
- Complexity: New infrastructure, operational burden
- Break point: millions/sec

Option C (Alternative): Redis streams
- Cost: $100/mo
- Complexity: Single machine, consumer groups work
- Break point: ~100k/sec

Recommendation: Start with PostgreSQL polling for proven reliability
and simplicity. Migrate to Kafka only if you hit 1k/sec limits and
operational overhead is acceptable.
