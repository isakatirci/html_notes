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
