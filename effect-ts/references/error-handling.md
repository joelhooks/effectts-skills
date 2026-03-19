# Error Handling

## Table of Contents

- [Schema.TaggedErrorClass](#schemataggederrorclass)
- [Yieldable Errors](#yieldable-errors)
- [Recovering from Errors](#recovering-from-errors)
- [Expected Errors vs Defects](#expected-errors-vs-defects)
- [Schema.Defect for Unknown Errors](#schemadefect-for-unknown-errors)

## Schema.TaggedErrorClass

Define domain errors with `Schema.TaggedErrorClass`:

```typescript
import { Schema } from "effect"

class ValidationError extends Schema.TaggedErrorClass("ValidationError")(
  "ValidationError",
  {
    field: Schema.String,
    message: Schema.String,
  }
) {}

class NotFoundError extends Schema.TaggedErrorClass("NotFoundError")(
  "NotFoundError",
  {
    resource: Schema.String,
    id: Schema.String,
  }
) {}

const AppError = Schema.Union([ValidationError, NotFoundError])
type AppError = typeof AppError.Type
```

**Benefits:**
- Serializable (can send over network, save to DB)
- Type-safe with built-in `_tag` for pattern matching
- Custom methods via class extension
- Sensible default `message` when you don't declare one

**Every distinct failure reason deserves its own error type.** Don't collapse multiple failure modes into generic errors like `NotFoundError`. Use `UserNotFoundError`, `ChannelNotFoundError`, etc. with relevant context fields.

## Yieldable Errors

`Schema.TaggedErrorClass` values are yieldable. Return them directly in generators without wrapping in `Effect.fail`:

```typescript
import { Effect, Random, Schema } from "effect"

class BadLuck extends Schema.TaggedErrorClass("BadLuck")(
  "BadLuck",
  { roll: Schema.Number }
) {}

const rollDie = Effect.gen(function* () {
  const roll = yield* Random.nextIntBetween(1, 6)
  if (roll === 1) {
    yield* new BadLuck({ roll }) // no Effect.fail needed
  }
  return { roll }
})
```

## Recovering from Errors

### catch

Handle all errors with a fallback:

```typescript
const recovered: Effect.Effect<string, never> = program.pipe(
  Effect.catch((error) =>
    Effect.gen(function* () {
      yield* Effect.logError("Error occurred", error)
      return `Recovered from ${error.name}`
    })
  )
)
```

### catchTag

Handle a specific error by its `_tag`:

```typescript
const recovered = program.pipe(
  Effect.catchTag("HttpError", (error) =>
    Effect.gen(function* () {
      yield* Effect.logWarning(`HTTP ${error.statusCode}: ${error.message}`)
      return "Recovered from HttpError"
    })
  )
)
// HttpError is removed from the error channel; other errors remain
```

### catchTags

Handle multiple error types at once:

```typescript
const recovered = program.pipe(
  Effect.catchTags({
    HttpError: () => Effect.succeed("Recovered from HttpError"),
    ValidationError: () => Effect.succeed("Recovered from ValidationError"),
  })
)
// Both error types removed from the error channel
```

## Expected Errors vs Defects

Effect tracks errors in the type system (`Effect<A, E, R>`) so callers know what can fail and can recover.

**Use typed errors** for domain failures the caller can handle: validation errors, "not found", permission denied, rate limits.

**Use defects** for unrecoverable situations: bugs, invariant violations. Defects terminate the fiber and you handle them once at the system boundary (logging, crash reporting).

```typescript
// At app entry: if config fails, nothing can proceed
const main = Effect.gen(function* () {
  const config = yield* loadConfig.pipe(Effect.orDie)
  yield* Effect.log(`Starting on port ${config.port}`)
})
```

**When to catch defects:** Almost never. Only at system boundaries for logging/diagnostics. Use `Effect.exit` to inspect or `Effect.catchAllDefect` if you must recover (e.g., plugin sandboxing).

## Schema.Defect for Unknown Errors

Wrap unknown errors from external libraries with `Schema.Defect`:

```typescript
import { Schema, Effect } from "effect"

class ApiError extends Schema.TaggedErrorClass("ApiError")(
  "ApiError",
  {
    endpoint: Schema.String,
    statusCode: Schema.Number,
    error: Schema.Defect, // wraps the underlying error
  }
) {}

const fetchUser = (id: string) =>
  Effect.tryPromise({
    try: () => fetch(`/api/users/${id}`).then((r) => r.json()),
    catch: (error) => new ApiError({
      endpoint: `/api/users/${id}`,
      statusCode: 500,
      error,
    }),
  })
```

**Schema.Defect handles:**
- JavaScript `Error` instances become `{ name, message }` objects
- Any unknown value becomes a string representation
- Result is serializable for network/storage

**Use for:** wrapping external library errors, network boundaries, persisting errors to DB, logging systems.
