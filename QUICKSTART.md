# Quick Start

1. Skills already live under `.claude/skills/`. In a new repo:

   ```bash
   mkdir -p .claude/skills
   ```

2. Start with Ask-Burda when the right skill is unclear:

   ```bash
   mkdir -p .claude/skills/ask-burda
   ```

   Copy `.claude/skills/ask-burda/SKILL.md`.

3. Or start with Blast Radius Analyzer:

   ```bash
   mkdir -p .claude/skills/blast-radius
   ```

   Copy `.claude/skills/blast-radius/SKILL.md`.

4. Point Claude Desktop at the skills path with `claude_desktop_config.json`:

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

5. Run a skill against a real change you are considering. Nothing gets implemented until the recommended skill has run.

## Workflow

**Unsure which skill to use**

```
You: "/ask-burda I'm redesigning our notification system"
Claude: [Routes you through questions] → Recommends 3-4 skills in order
```

See `.claude/skills/ask-burda/examples/routing.md`.

**Before code changes**

```
You: "I need to change the user authentication flow"
Claude: [Uses blast-radius] "Here's every service that depends on
current auth behavior, organized by risk level"
```

**When tests fail**

```
You: "This test passes locally but fails in CI"
Claude: [Uses root-cause] "Hypothesis 1: Race condition because
CI has 8 cores. Here's the test that would prove it..."
```

**When planning architecture**

```
You: "We need a microservices architecture"
Claude: [Uses architecture-challenger] "What problem are you solving?
Let's compare against monolith with separate databases..."
```

**Before shipping**

```
You: "Feature is ready for production"
Claude: [Uses production-paranoia] "Here are 7 ways this could fail
in production and what breaks..."
```

## Skill chains

1. Blast Radius (what breaks?)
2. Adversarial Tests (edge cases)
3. Production Paranoia (before shipping)

Or:

1. Architecture Challenger (design decision)
2. Migration Strategist (rollout)
3. Production Paranoia (failure scenarios)
