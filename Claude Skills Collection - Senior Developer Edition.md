# Claude Skills Collection: Senior Developer Edition

A production-ready collection of Claude MCP skills that elevate your AI agent from junior to senior developer. Each skill forces deliberate thinking before implementation.

---

## 🧭 Skill 0: Ask-Burda (Routing Skill)

Not sure which skill fits your situation? Start here.

**When to use:** You're about to make a change but unsure which skill will help most.

**What it does:** Asks targeted questions about your task, then routes you to the most valuable skill or combination of skills.

### Configuration

**File:** `.claude/skills/ask-burda/SKILL.md`

```yaml
---
name: ask-burda
description: Route to the right skill based on your task type and concerns.
---
# Ask-Burda: Skill Router

## Purpose
When you're unsure which skill to use, ask this first. I'll interview you about your task and route you to the most valuable skill or combination.

## The Questions I Ask

1. **What are you trying to do?**
   - Add a feature / fix a bug / refactor / deploy
   - Make an architectural decision / optimize performance
   - Debug a failure / migrate data / prepare for production

2. **What's your biggest concern right now?**
   - Will this break other services?
   - Why is this test failing or system slow?
   - Is this the right design choice?
   - Did I miss any edge cases?
   - How do I deploy this safely?
   - Will this survive production?

3. **Is this changing existing behavior or adding new?**
   - Pure addition (safer)
   - Modification (blast radius concerns)
   - Removal (migration complexity)

4. **Who else depends on this code?**
   - Just this service
   - Multiple internal services
   - External customers / mobile clients
   - Long-running background jobs

5. **What's the highest-risk part of this task?**
   - Correctness (will it work?)
   - Performance (will it be fast?)
   - Compatibility (will it break things?)
   - Production readiness (will it survive?)
   - Deployment safety (can I roll back?)

## Routing Logic

**"I'm changing an API/schema/function signature"**
→ **Blast Radius Analyzer** (find who depends on this)

**"This test keeps failing and I don't know why"**
→ **Root Cause Detective** (form hypotheses, test them)

**"Should we use Kafka/microservices/caching?"**
→ **Architecture Challenger** (compare alternatives)

**"I need solid tests for this feature"**
→ **Adversarial Test Engineer** (find edge cases)

**"This endpoint is too slow"**
→ **Performance Archaeologist** (find the bottleneck)

**"How do I deploy this without breaking things?"**
→ **Migration Strategist** (expand-migrate-contract)

**"Is this feature production-ready?"**
→ **Production Paranoia** (identify failure modes)

## After Routing

I'll show you:
- ✅ Why this skill fits your situation
- ✅ The exact process it uses
- ✅ A concrete example from similar work
- ✅ What to expect from the skill
- ✅ When to loop back and use a different skill

**Nothing gets implemented until you run the recommended skill.**

## When to Use Multiple Skills

Sometimes one skill leads to another:
1. Use **Blast Radius** first (what breaks?)
2. Then **Adversarial Tests** (will it handle edge cases?)
3. Then **Production Paranoia** (before shipping)

Or:
1. **Architecture Challenger** (design decision)
2. **Migration Strategist** (how to roll out)
3. **Production Paranoia** (failure scenarios)

I'll tell you when to chain skills together.
```

### Usage Example

```
You: "/ask-burda I'm redesigning our notification system"

Me: Asking questions...
- What's the main concern? (scalability, reliability, deployment)
- Who consumes this system? (internal services, external, mobile)
- Are you changing the API contract?
- What's the biggest risk?

Based on your answers, I route you to:
1. **Architecture Challenger** — Compare monolith vs. queue vs. serverless
2. **Blast Radius Analyzer** — Find services depending on current API
3. **Migration Strategist** — Roll out changes without breaking consumers
4. **Production Paranoia** — Review failure modes before deployment

You run each skill in order, learning more as you go.
```

---

## 📋 Skills Overview

| Skill | Purpose | When to Use |
|-------|---------|-----------|
| **Ask-Burda** | Route to the right skill | Unsure which skill to use |
| **Blast Radius Analyzer** | Identify downstream impact before code changes | API changes, function renames, schema modifications |
| **Root Cause Detective** | Investigate failures before applying fixes | Flaky tests, regressions, runtime errors |
| **Architecture Challenger** | Challenge design decisions with alternatives | New services, major refactors, infrastructure |
| **Adversarial Test Engineer** | Generate edge cases from requirements | APIs, state machines, concurrency features |
| **Performance Archaeologist** | Find bottlenecks before optimizing | Slow requests, memory leaks, expensive queries |
| **Migration Strategist** | Plan backward-compatible deployments | Schema changes, API versioning, data migrations |
| **Production Paranoia** | Review failure modes before shipping | Critical features, worker jobs, payment flows |

---

## 🚀 Quick Start

1. **Initialize your skills directory:**
   ```bash
   mkdir -p .claude/skills
   ```

2. **Start with Ask-Burda** (when unsure which skill to use):
   ```bash
   mkdir -p .claude/skills/ask-burda
   # Copy the skill configuration above
   ```

3. **Or start directly with Blast Radius Analyzer**:
   ```bash
   mkdir -p .claude/skills/blast-radius
   # Copy the skill configuration below
   ```

4. **Configure Claude** to use the skill in your `claude_desktop_config.json`

5. **Test it** on a real change you're considering

---

## 💡 Skill 1: Blast Radius Analyzer

**Problem:** Confident changes that silently break three other services.

**Solution:** Trace dependencies before touching implementation.

### Configuration

**File:** `.claude/skills/blast-radius/SKILL.md`

```yaml
---
name: blast-radius
description: Analyze downstream impact before changing APIs,
  shared interfaces, database schemas, or core logic.
---
# Blast Radius Analyzer

## Purpose
Before implementing a change to any interface, contract, or shared behavior, identify everything that depends on the current implementation.

## Process

1. **Map the Interface**
   - Identify every interface or contract affected
   - List public methods, API endpoints, database fields, message formats
   - Find all exported symbols and their visibility scope

2. **Trace Direct Callers**
   - Code references (grep for function/method names)
   - Service-to-service calls (API clients, SDK usage)
   - Background job references
   - Cache invalidation points

3. **Trace Indirect Consumers**
   - API consumers and SDK users
   - Database queries that depend on schema
   - Scheduled jobs that read the output
   - Event streams and message brokers
   - Mobile clients and older deployments

4. **Classify Dependencies**
   - ✓ **Confirmed breakage**: Direct reference with no adapter
   - ⚠ **Possible risk**: Indirect dependency or inferred usage
   - ✓ **Unaffected**: No dependency found
   - ❓ **Unknown**: Suspected usage but no evidence

5. **Evidence & Recommendations**
   - Cite file paths and line numbers
   - Provide symbol names and type signatures
   - List test coverage for each dependency
   - Recommend smallest safe implementation
   - Specify tests needed to verify safety

## Guidelines

- ✅ DO trace dependencies systematically
- ✅ DO separate verified from suspected risks
- ✅ DO cite evidence (file paths, symbols, line numbers)
- ✅ DO recommend backward-compatible approaches
- ❌ DO NOT modify code during analysis
- ❌ DO NOT invent dependencies without evidence
- ❌ DO NOT continue if blast radius is unclear
```

### Usage Example

```
You: "I want to rename the `getUserById()` function to `fetchUser()`"

Blast Radius: 
- Direct callers in UserService: 23 references (file:line provided)
- REST API endpoint `/users/:id` depends on this
- Mobile app v1.0-1.2 makes calls (3 months to update)
- Background job `UserNotifier` calls this at startup
- Tests: 8 tests verify behavior, all will break with rename

Recommendation: Add new `fetchUser()`, deprecate `getUserById()`, 
run both for 2 releases, update tests incrementally.
```

---

## 🔍 Skill 2: Root Cause Detective

**Problem:** Finding something that looks like a bug and stopping there (timeouts, flakes, symptom-fixes).

**Solution:** Form competing hypotheses and test each one.

### Configuration

**File:** `.claude/skills/root-cause/SKILL.md`

```yaml
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
```

### Usage Example

```
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
```

---

## 🏗️ Skill 3: Architecture Challenger

**Problem:** Coding agents agree with every proposed architecture because they assume it's part of the requirement.

**Solution:** Challenge with simpler alternatives and compare thoroughly.

### Configuration

**File:** `.claude/skills/architecture-challenger/SKILL.md`

```yaml
---
name: architecture-challenger
description: Challenge architectural proposals before
  implementation. Use for new services, infrastructure,
  abstractions, or major design decisions.
---
# Architecture Challenger

## Purpose
Act as a skeptical senior engineer. Ensure proposed solutions earn their complexity by comparing against simpler alternatives.

## Process

1. **Restate the Problem**
   - Extract requirements without the proposed solution
   - Ignore suggested technologies
   - Focus on constraints and goals
   - Document scaling, reliability, and consistency needs

2. **Identify Hidden Assumptions**
   - Scale assumptions (users, requests/sec, data volume)
   - Reliability requirements (uptime, failover)
   - Consistency needs (strong, eventual)
   - Operational constraints (teams, budget, dependencies)

3. **Compare Three Approaches**

   **Option A: Simplest Viable Solution**
   - Minimum code and infrastructure
   - Achieves the core requirement
   - Cost and complexity baseline

   **Option B: Proposed Solution**
   - Original suggestion
   - Full architectural design
   - As specified

   **Option C: Meaningfully Different Alternative**
   - Different philosophy or trade-off
   - Still viable for the problem
   - Contrasts with both others

4. **Evaluate Each Approach**
   - **Complexity**: Lines of code, infrastructure, mental overhead
   - **Failure modes**: What breaks and what's the impact?
   - **Operational cost**: Maintenance, monitoring, learning curve
   - **Scalability limits**: Where does each approach break?
   - **Reversibility**: Can you change your mind later?
   - **Time to production**: When is each ready?

5. **Recommend**
   - Simplest sufficient design
   - Conditions where other approaches win
   - Which assumptions would change the decision

## Guidelines

- ✅ DO evaluate alternatives seriously
- ✅ DO question scale assumptions
- ✅ DO identify when simple is enough
- ✅ DO explain failure modes for each
- ❌ DO NOT invent requirements to justify complexity
- ❌ DO NOT agree without comparison
- ❌ DO NOT select "senior sounding" solutions
```

### Usage Example

```
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
```

---

## ⚔️ Skill 4: Adversarial Test Engineer

**Problem:** Tests pass because they encode the same assumptions as the code.

**Solution:** Derive tests from requirements, not implementation. Hunt hostile inputs.

### Configuration

**File:** `.claude/skills/adversarial-tests/SKILL.md`

```yaml
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
```

### Usage Example

```
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
```

---

## ⚡ Skill 5: Performance Archaeologist

**Problem:** Optimization without measurement. Caching, parallelization, rewrites without knowing if they help.

**Solution:** Find the bottleneck first. Measure before and after.

### Configuration

**File:** `.claude/skills/performance-archaeologist/SKILL.md`

```yaml
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
```

### Usage Example

```
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
```

---

## 🚀 Skill 6: Migration Strategist

**Problem:** Changes work locally but break during rolling deployment when old and new code coexist.

**Solution:** Plan expand-migrate-contract. Design for in-flight deployments.

### Configuration

**File:** `.claude/skills/migration-strategist/SKILL.md`

```yaml
---
name: migration-strategist
description: Plan backward-compatible database, API,
  and data-format migrations for rolling deployments
  and mixed-version systems.
---
# Migration Strategist

## Purpose
Assume old and new versions will run simultaneously. Design migrations as a sequence of safe phases.

## Process

1. **Map Current State**
   - Existing contracts (API, schema, message format)
   - All readers: services, scripts, clients
   - All writers: services, jobs, migrations
   - Deployment schedule and rollback window

2. **Map Desired State**
   - New contract requirements
   - Breaking changes unavoidable?
   - New readers, writers, dependencies

3. **Design Expand-Migrate-Contract**

   **Phase 1: Expand** (Old and new coexist)
   - Add new fields/endpoints/formats
   - New code reads both old and new
   - Old code continues unchanged
   - Backward compatible, never breaks

   **Phase 2: Migrate** (Move consumers)
   - Backfill old data to new format
   - Update writers to use new format
   - Dual-write during transition
   - Consumers migrate one by one

   **Phase 3: Contract** (Remove old)
   - Stop writing old format
   - Verify all readers updated
   - Remove old fields/endpoints
   - Only after safe period

4. **Handle Deployment Challenges**
   - **Mid-flight crashes**: What if process dies during phase 2?
   - **Rollbacks**: Can you go back to previous version?
   - **Long-running requests**: Will they see old or new?
   - **Mobile clients**: Can they lag for months?
   - **Dual writes**: How to keep them consistent?
   - **Data consistency**: What if backfill is incomplete?

5. **Define Verification Gates**
   - What metrics prove a phase is complete?
   - Monitoring and alerting between phases
   - Rollback triggers and procedures
   - "Safe to proceed" checklist

6. **Production Checklist**
   - Feature flags for easy rollback
   - Monitoring for old format usage
   - Alerting for writer failures
   - Gradual rollout procedure (canary)

## Guidelines

- ✅ DO plan phases, not atomic changes
- ✅ DO support mixed versions
- ✅ DO test incomplete deployments
- ✅ DO provide rollback at each phase
- ✅ DO monitor old format usage
- ❌ DO NOT assume instant updates
- ❌ DO NOT break old clients immediately
- ❌ DO NOT remove old contracts until proven safe
- ❌ DO NOT skip verification gates
```

### Usage Example

```
Change: Rename user table columns (old_name → new_name)

Phase 1 - Expand (1 release):
- Add new_name column, backfill from old_name
- Write to both old_name and new_name
- Read from new_name (fallback to old_name)
- Schema: Both columns present

Phase 2 - Migrate (2-3 releases):
- Update all readers to use new_name
- Stop writing to old_name
- Monitor old_name queries (should drop)
- Schema: Both columns, old_name unused

Phase 3 - Contract (1 release):
- Verify no reads of old_name (monitoring)
- Drop old_name column
- Update schema
- Schema: Only new_name

Safety: Each phase deployable independently. Rollback: Drop 
new column, revert writes. Mobile lag: Supported via fallback reads.
```

---

## 🛡️ Skill 7: Production Paranoia

**Problem:** Features work perfectly locally, fail mysteriously in production.

**Solution:** Review failure modes before shipping. Assume worst.

### Configuration

**File:** `.claude/skills/production-paranoia/SKILL.md`

```yaml
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
```

### Usage Example

```
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
```

---

## 📦 Repository Structure

```
your-repo/
├── .claude/
│   └── skills/
│       ├── ask-burda/
│       │   ├── SKILL.md
│       │   └── examples/
│       │       └── routing.md
│       ├── blast-radius/
│       │   ├── SKILL.md
│       │   └── examples/
│       │       └── api-change.md
│       ├── root-cause/
│       │   ├── SKILL.md
│       │   └── examples/
│       │       └── flaky-test.md
│       ├── architecture-challenger/
│       │   ├── SKILL.md
│       │   └── examples/
│       │       └── kafka-decision.md
│       ├── adversarial-tests/
│       │   ├── SKILL.md
│       │   └── examples/
│       │       └── payment-api.md
│       ├── performance-archaeologist/
│       │   ├── SKILL.md
│       │   └── examples/
│       │       └── slow-query.md
│       ├── migration-strategist/
│       │   ├── SKILL.md
│       │   └── examples/
│       │       └── schema-rename.md
│       └── production-paranoia/
│           ├── SKILL.md
│           └── examples/
│               └── payment-processing.md
├── README.md
├── QUICKSTART.md
└── claude_desktop_config.json
```

---

## 🔧 Claude Desktop Configuration

**File:** `claude_desktop_config.json`

```json
{
  "mcpServers": {
    "skills": {
      "command": "node",
      "args": ["skill-runner.js"],
      "env": {
        "SKILLS_PATH": "./.claude/skills"
      }
    }
  }
}
```

---

## 💻 Integration with Your Workflow

### Unsure which skill to use?
```
You: "/ask-burda I'm redesigning our notification system"
Claude: [Routes you through questions] → Recommends 3-4 skills in order
```

### Before Code Changes
```
You: "I need to change the user authentication flow"
Claude: [Uses blast-radius] "Here's every service that depends on 
current auth behavior, organized by risk level"
```

### When Tests Fail
```
You: "This test passes locally but fails in CI"
Claude: [Uses root-cause] "Hypothesis 1: Race condition because 
CI has 8 cores. Here's the test that would prove it..."
```

### When Planning Architecture
```
You: "We need a microservices architecture"
Claude: [Uses architecture-challenger] "What problem are you solving? 
Let's compare against monolith with separate databases..."
```

### Before Shipping
```
You: "Feature is ready for production"
Claude: [Uses production-paranoia] "Here are 7 ways this could fail 
in production and what breaks..."
```

---

## 🎯 Which Skill to Start With?

**Best first skill:** Ask-Burda
- Routes you intelligently based on your task
- Works with any other skill
- No special setup required
- Learns your patterns

**Alternative first skill:** Blast Radius Analyzer
- Easy to see immediate value
- Works on any codebase
- No special setup required
- Makes patterns obvious quickly

**Most impactful skill:** Production Paranoia
- Prevents costly bugs
- Catches design assumptions
- Gets better with practice
- Team learns failure modes

**Quickest wins:** Adversarial Test Engineer
- Finds real bugs immediately
- Improves test quality
- Works on existing code
- Prevents regressions

---

## 📚 Reading List

- **Debugging**: "Debugging: The 9 Indispensable Rules for Finding Even the Most Elusive Software and Hardware Problems" - David Agans
- **Distributed Systems**: "Designing Data-Intensive Applications" - Martin Kleppmann
- **Testing**: "Growing Object-Oriented Software, Guided by Tests" - Steve Freeman
- **Performance**: "Systems Performance" - Brendan Gregg
- **Deployment**: "Release It!" - Michael Nygard

---

## 💬 Contributing Skills

If you develop a new skill that helps your Claude agent think like a senior engineer, consider contributing it:

1. Create `.claude/skills/your-skill/SKILL.md`
2. Include process, guidelines, and usage example
3. Add real example from your codebase
4. Test with Claude and iterate
5. Open a pull request

---

## License

MIT — Use these skills freely in your projects. Share improvements.

---

**Last Updated:** October 2026  
**Claude Model:** opus-5.5  
**Status:** Production Ready