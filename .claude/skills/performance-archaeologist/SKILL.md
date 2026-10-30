---
name: performance-archaeologist
description: Diagnose slow requests, expensive queries,
  high memory usage, or performance regressions
  before proposing optimizations.
---
# Performance Archaeologist

## Purpose
Never optimize without locating the bottleneck. Measure, find evidence, rank fixes by impact, then propose only the changes that matter.

## Process

1. **Define the Problem**
   - Workload profile (peak requests/sec, data size)
   - Target metric (response time, throughput, memory)
   - Acceptable threshold and current performance
   - Cost of optimization vs. cost of slowness

2. **Collect Evidence**
   - Profiling data (CPU, memory, I/O)
   - Traces and request logs
   - Database query plans and slow query logs
   - Benchmark results (before, during, after)
   - Availability of monitoring and APM tools

3. **Investigate Bottlenecks**

   **N+1 Queries**
   - List queries in trace, look for patterns
   - Check for loops fetching data
   - Verify batch operations exist
   - Count query multiplier

   **I/O Hotspots**
   - Network calls in loops
   - Repeated cache misses
   - Disk seeks in sequences
   - Data serialization overhead

   **Memory Pressure**
   - Allocation patterns
   - Garbage collection pauses
   - Cache thrashing
   - Object retention

   **Algorithmic Complexity**
   - O(n²) operations on large sets
   - Repeated work in loops
   - Inefficient data structures
   - Sorting when filtering sufficient

   **Lock Contention**
   - Shared locks on hot paths
   - Long critical sections
   - Thread pool exhaustion
   - Deadlock candidates

4. **Separate Measurements from Hypotheses**
   - Profile data vs. guesses
   - Time measurements vs. "probably slow"
   - Verified bottlenecks vs. suspected ones

5. **Rank Fixes**
   - Expected impact (how much faster?)
   - Risk (will this break anything?)
   - Effort (time to implement)
   - Create 2-3 top candidates

6. **Define Success Metrics**
   - Before benchmark (measured)
   - After benchmark (target)
   - Regression test (prevent reversion)

## Guidelines

- ✅ DO profile before optimizing
- ✅ DO measure improvements
- ✅ DO rank by impact
- ✅ DO optimize algorithmic complexity first
- ✅ DO test that optimization actually helps
- ❌ DO NOT invent performance numbers
- ❌ DO NOT optimize by taste
- ❌ DO NOT claim speedups without measurement
- ❌ DO NOT blindly add caching
