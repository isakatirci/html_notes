# Claude Skills Collection: Senior Developer Edition

A production-ready collection of Claude skills that elevate an AI agent from junior to senior developer. Each skill forces deliberate thinking before implementation.

## Skills

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **Ask-Burda** | Route to the right skill | Unsure which skill to use |
| **Blast Radius Analyzer** | Identify downstream impact before code changes | API changes, function renames, schema modifications |
| **Root Cause Detective** | Investigate failures before applying fixes | Flaky tests, regressions, runtime errors |
| **Architecture Challenger** | Challenge design decisions with alternatives | New services, major refactors, infrastructure |
| **Adversarial Test Engineer** | Generate edge cases from requirements | APIs, state machines, concurrency features |
| **Performance Archaeologist** | Find bottlenecks before optimizing | Slow requests, memory leaks, expensive queries |
| **Migration Strategist** | Plan backward-compatible deployments | Schema changes, API versioning, data migrations |
| **Production Paranoia** | Review failure modes before shipping | Critical features, worker jobs, payment flows |

## Repository structure

```
.
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

Setup steps are in [QUICKSTART.md](QUICKSTART.md).

## Which skill to start with

**Best first skill:** Ask-Burda. It routes from the task and chains into the others.

**Alternative:** Blast Radius Analyzer. Immediate value on any codebase.

**Most impactful:** Production Paranoia. Catches failure modes before shipping.

**Quickest wins:** Adversarial Test Engineer. Finds edge cases in existing behavior.

## Contributing

1. Create `.claude/skills/your-skill/SKILL.md`
2. Include process, guidelines, and a usage example
3. Add a real example under `examples/`
4. Test with Claude and iterate
5. Open a pull request

## License

MIT — Use these skills freely in your projects. Share improvements.
