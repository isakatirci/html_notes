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
