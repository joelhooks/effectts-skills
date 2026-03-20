# ⚡ Effect-TS Agent Skill

Comprehensive [Effect](https://effect.website) v4 skill for AI coding agents. Covers services, layers, data modeling, error handling, testing, HTTP clients, CLI tools, config, scopes, and project setup.

## Install

```bash
npx skills add joelhooks/effectts-skills
```

Or manually copy the `effect-ts/` directory into your agent's skills folder (`.claude/skills/`, `.pi/agent/skills/`, etc.).

## What's Included

| File | Topics |
|------|--------|
| `effect-ts/SKILL.md` | Core patterns: ServiceMap.Service, Effect.fn/gen, Schema.Class, TaggedErrorClass, Layer composition, anti-patterns |
| `references/services-and-layers.md` | Service-driven development, Layer.effect/sync, test layers, memoization, provide vs provideMerge |
| `references/data-modeling.md` | Schema.Class, branded types, TaggedClass variants, Match.valueTags, JSON encoding |
| `references/schema-decisions.md` | Schema.Class vs Struct vs TaggedClass decision flowchart, migration patterns |
| `references/error-handling.md` | TaggedErrorClass, yieldable errors, catch/catchTag/catchTags, defects, TypeId/refail |
| `references/testing.md` | @effect/vitest, it.effect/live/layer, TestClock, Effect.flip, FiberRef isolation, worked example |
| `references/http-clients.md` | effect/unstable/http, requests, responses, middleware, retries, typed API service |
| `references/cli.md` | effect/unstable/cli, Arguments, Flags, subcommands, task manager example |
| `references/config.md` | Config service pattern, schema validation, ConfigProvider, Redacted secrets |
| `references/processes.md` | Fork types, Scope.extend, Command for child processes, killable background tasks |
| `references/setup.md` | tsconfig, Effect Language Service, module settings, dev workflow |

## Sources

Built from three excellent repos:

- **[kitlangton/effect-solutions](https://github.com/kitlangton/effect-solutions)** - Effect best practices and patterns by [Kit Langton](https://github.com/kitlangton). The primary source for idiomatic v4 patterns, worked examples, and opinionated guidance. Also available at [effect.solutions](https://effect.solutions).
- **[effect-ts/effect](https://github.com/effect-ts/effect)** - The canonical Effect monorepo. Source for all `@effect/*` packages, API definitions, and development workflow conventions.
- **[artimath/effect-skills](https://github.com/artimath/effect-skills)** (MIT) - Effect agent skills by [Artimath](https://github.com/artimath). Source for the schema decision matrix, process/scope management patterns, layer dependency gotchas, TypeId/refail error patterns, and FiberRef test isolation. Also includes the SDD (Service-Driven Development) and deep-audit skills which are worth checking out separately.

## License

MIT
