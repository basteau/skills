# Better modeling

Choose types that describe valid domain values and make callers handle real
possibilities. Strengthen a model when doing so removes a demonstrated ambiguity,
unchecked access, or repeated invariant check.

## Represent alternatives explicitly

Use a discriminated union when fields depend on a state. Independent optional
fields are appropriate for genuinely independent choices, not mutually exclusive
outcomes. Keep the project's existing discriminant vocabulary.

```ts
type Load<T> =
  | { kind: "pending" }
  | { kind: "ready"; value: T }
  | { kind: "failed"; message: string };

function describeLoad<T>(state: Load<T>): string {
  switch (state.kind) {
    case "pending": return "Loading";
    case "ready": return String(state.value);
    case "failed": return state.message;
    default: {
      const unreachable: never = state;
      throw new Error(`Unexpected state: ${String(unreachable)}`);
    }
  }
}
```

Exhaustiveness makes a new variant visible to the compiler. It does not validate
incoming JSON or prove that transitions between otherwise valid states are legal.
Model transition rules separately when the domain requires them.

## Choose a total contract

Keep ordinary arrays when empty input has a defined result. For a required first
element, either accept a non-empty tuple or return a possibly absent result.

```ts
type NonEmpty<T> = readonly [T, ...T[]];

function first<T>(items: NonEmpty<T>): T {
  return items[0];
}

function optionalFirst<T>(items: readonly T[]): T | undefined {
  return items[0];
}
```

A non-empty tuple proves index zero exists, not that an arbitrary numeric index
is in bounds. Under `noUncheckedIndexedAccess`, dynamic access can still return
`undefined`. Do not cast it away. Prefer iteration where indexing adds no value.
Readonly views prevent writes through that view; mutable aliases can still change
the underlying object. Copy or control ownership when an invariant must persist.

## Brand only distinctions that matter

Use the project's branding convention to prevent meaningful identifier or unit
mix-ups. A brand is compile-time evidence attached by trusted construction; it does
not validate input, add a runtime tag, enforce authorization, or protect a secret.

```ts
declare const durationBrand: unique symbol;
type DurationMs = number & { readonly [durationBrand]: true };

function parseDurationMs(value: number): DurationMs {
  if (!Number.isFinite(value) || value < 0) {
    throw new Error("Expected a finite, non-negative duration");
  }
  // The constructor establishes the invariant represented by this brand.
  return value as DurationMs;
}
```

Replacing `{ start, end }` with `{ start, duration }` can simplify a range, but a
plain `number` still permits negative values, infinity, and `NaN`. Validate actual
domain constraints at construction. Do not brand every string or number by habit.

## Preserve useful distinctions

- Missing, `undefined`, and `null` can mean different things in patches and wire
  formats. Define omission and clearing semantics instead of using `Partial<T>`
  as a universal update contract.
- A finite `Record<KeyUnion, Value>` requires those keys. An open string dictionary
  does not mean every lookup exists; include absence or use a suitable map.
- TypeScript object types are structural, not exact object schemas. Excess-property
  checks on fresh literals do not reject all extra properties arriving via variables.
- Reuse a generated shape when it owns the contract. Introduce a separate domain
  type when the application intentionally transforms or decouples the wire model.

Adapted from [poteto's TypeScript practices](https://github.com/backnotprop/pstack/tree/main/skills/typescript-best-practices)
and [Matteo Collina's type patterns](https://github.com/mcollina/skills/tree/main/skills/typescript-magician).
See [better-boundaries](better-boundaries.md) for construction and validation.
