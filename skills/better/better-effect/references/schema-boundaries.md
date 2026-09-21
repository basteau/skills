# Schema boundaries

Decode untrusted data where it enters: HTTP, environment/config, database records,
external SDK results, tool arguments, and persisted messages. Keep transport
representation separate from the validated domain value. An assertion, brand cast,
or constructor invocation is not proof that unknown input was decoded.

Choose `Schema.Struct` for records or `Schema.Class`/`TaggedClass` when class
construction/behavior fits the project. Both are legitimate. Derive types from
the schema where practical; avoid maintaining a second, divergent wire contract.
Brands distinguish validated IDs or values; establish them through validation.

Represent mutually exclusive domain states with tagged unions rather than several
booleans or optional fields that permit impossible combinations. Handle variants
exhaustively with a switch or `Match`. Use `Option` for meaningful absence, `Result`
for a pure success/failure value, and Effect when computation needs effects,
services, or lifecycle. Convert between these explicitly at boundaries.

Choose equality intentionally for IDs, models, and collection keys. Plain objects
and structurally identical values are not interchangeable under every equality
operation. Inspect `Equal`/`Hash` when domain values become HashMap or HashSet keys.

Before selecting an API, identify:

- Decoded type versus encoded type, and which direction is being used.
- Decode/encode service requirements and the enclosing effect's `R`.
- Missing property versus explicit `undefined`, `null`, and default insertion.
- Failure representation and which caller can recover from it.

Use an effectful decoder such as `Schema.decodeUnknownEffect(schema)`. Confirm the
release's actual sync/Effect and encode APIs as appropriate; do not mix historical
ParseResult transforms with current SchemaGetter/SchemaTransformation recipes.
Search the Schema manual for the precise filter, optional-key, default, or
transformation operation instead of reading the whole manual.

When changing a codec, test both directions with meaningful boundary values.
Roundtrip expectations can involve normalization; byte-for-byte equality is not
always the contract. For a PATCH field, test absent (leave unchanged), null (clear,
if allowed), and supplied value separately. Check whether excess properties are
preserved, stripped, or rejected when this affects the transport contract.

Schema-backed errors can cross boundaries, but internal causes, credentials, or
stack traces may need a separate public representation. JSON Schema/OpenAPI
conversion does not automatically preserve every runtime refinement or transform.

Lookup: `src/Schema.ts`, `src/SchemaIssue.ts`, `src/SchemaGetter.ts`,
`src/SchemaTransformation.ts`; the matching upstream `packages/effect/SCHEMA.md`.
