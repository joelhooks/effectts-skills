# ⚡ Effect-TS Agent Skill

Comprehensive [Effect](https://effect.website) v4 skill for AI coding agents. Covers services, layers, data modeling, error handling, testing, HTTP clients, CLI tools, config, and project setup.

## Install

```bash
npx skills add joelhooks/effectts-skills
```

Or manually copy the `effect-ts/` directory into your agent's skills folder (`.claude/skills/`, `.pi/agent/skills/`, etc.).

## What's Included

| File | Topics |
|------|--------|
| `effect-ts/SKILL.md` | Core patterns: ServiceMap.Service, Effect.fn/gen, Schema.Class, TaggedErrorClass, Layer composition, anti-patterns |
| `references/services-and-layers.md` | Service-driven development, Layer.effect/sync, test layers, memoization |
| `references/data-modeling.md` | Schema.Class, branded types, TaggedClass variants, Match.valueTags, JSON encoding |
| `references/error-handling.md` | TaggedErrorClass, yieldable errors, catch/catchTag/catchTags, defects, Schema.Defect |
| `references/testing.md` | @effect/vitest, it.effect/live/layer, TestClock, full worked example |
| `references/http-clients.md` | effect/unstable/http, requests, responses, middleware, retries, typed API service |
| `references/cli.md` | effect/unstable/cli, Arguments, Flags, subcommands, task manager example |
| `references/config.md` | Config service pattern, schema validation, ConfigProvider, Redacted secrets |
| `references/setup.md` | tsconfig, Effect Language Service, module settings, dev workflow |

## Sources

Built from two excellent repos:

- **[kitlangton/effect-solutions](https://github.com/kitlangton/effect-solutions)** - Effect best practices and patterns by [Kit Langton](https://github.com/kitlangton). The primary source for idiomatic v4 patterns, worked examples, and opinionated guidance. Also available at [effect.solutions](https://effect.solutions).
- **[effect-ts/effect](https://github.com/effect-ts/effect)** - The canonical Effect monorepo. Source for all `@effect/*` packages, API definitions, and development workflow conventions.

## License

MIT
