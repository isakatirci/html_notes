# Example: User profile page takes 2 seconds

Complaint: "User profile page takes 2 seconds, should be 500ms"

Measurements (APM):
- API call: 1800ms (90%)
  - Database: 1600ms (Query A and B, 5 queries total)
  - Serialization: 150ms
  - Network: 50ms

Root: Query A returns 10k rows, Query B runs 10 times (N+1)

Fixes ranked by impact:
1. Fix N+1: Batch Query B → ~400ms total (80% improvement, low risk)
2. Add database index → ~1400ms total (30%, moderate risk)
3. Caching → ~200ms total (90%, high risk - invalidation)

Recommendation: Fix N+1 first. Measure. Only cache if still slow.
