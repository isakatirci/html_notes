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
