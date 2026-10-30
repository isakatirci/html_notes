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
