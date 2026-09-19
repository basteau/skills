# Better generics

A generic earns its complexity by preserving a relationship between inputs,
outputs, or stored state. If callers gain nothing over a concrete type or union,
remove the type parameter. Constrain only what the implementation needs.

## Preserve relationships, not just shapes

```ts
function getProperty<T, K extends keyof T>(object: T, key: K): T[K] {
  return object[key];
}

const account = { name: "Ada", active: true };
const name = getProperty(account, "name"); // string
// @ts-expect-error: the key must exist
getProperty(account, "missing");
```

Put a type parameter where inference can see its value. A return-only generic
cannot manufacture a value of the caller's chosen type. A generic constrained to
`{ id: string }` may have additional required fields; constructing only `{ id }`
does not produce an arbitrary `T`.

Accept `readonly T[]` when only reading. Use `const` type parameters for APIs that
need literal configuration detail, with readonly-compatible constraints. They do
not recover literals already widened in a variable. Prefer built-in inference
before introducing a deep-narrowing library.

When one argument defines the allowed values and another must be checked against
them, stop the second argument from widening the inferred choice:

```ts
function choose<C extends string>(choices: readonly C[], fallback: NoInfer<C>): C {
  return choices[0] ?? fallback;
}

choose(["small", "large"], "small");
// @ts-expect-error: fallback cannot add a new choice
choose(["small", "large"], "medium");
```

Check compiler support before using `const` type parameters or `NoInfer`; preserve
the project's supported version rather than upgrading it to accommodate an example.

## Keep correlated arguments together

A union of keys and a separate union of values lose their pairing. A generic
`K` can also itself be a union. For a closed set of valid call combinations,
consider a discriminated object or a union of argument tuples:

```ts
type Notices = {
  saved: { id: string };
  retried: { attempt: number };
};
type NoticeArgs = {
  [K in keyof Notices]: [kind: K, value: Notices[K]];
}[keyof Notices];

function describeNotice(...args: NoticeArgs): string {
  switch (args[0]) {
    case "saved": return args[1].id;
    case "retried": return `Attempt ${args[1].attempt}`;
  }
}

describeNotice("saved", { id: "a1" });
// @ts-expect-error: this payload belongs to a different notice
describeNotice("saved", { attempt: 2 });
```

## Choose unions, generics, or overloads

- Use a union when behavior accepts several inputs but the result has one contract.
- Use generics when the result or callback depends on an input's type.
- Use overloads for genuinely distinct call forms. Put specific signatures before
  broad ones, and expose a union overload only when union callers are supported.

The overload implementation signature is not a public call signature. Compatibility
checks do not prove that each runtime branch returns the value promised by its
overload. Test the branches. `Parameters` and `ReturnType` do not reconstruct the
whole overload set; extraction normally reflects its last signature.

Function parameter variance matters: a callback accepting only strings cannot
handle arbitrary `unknown` values. Do not mechanically replace a type-level
`(...args: any[]) => unknown` constraint with `unknown[]`. Prefer a captured argument
tuple when calling a function. A deliberately permissive type-only constraint can
be valid; keep value-level `any` from escaping into inputs and results.

## Make builders prove their advertised state

Prefer an ordinary options object unless a staged API provides useful guarantees.
If using a builder, make its generic state observable in its members or signatures
so incomplete and complete builders are actually distinguishable. Test early
`build()`, branching from an intermediate value, and repeated setters.

Mutable shared state can invalidate a previously typed builder alias. Prefer
immutable transitions when branching is supported. An intersection is not a model
of overwriting: setting a property twice can yield an impossible `never` property
even though JavaScript simply replaces it. Choose replacement semantics deliberately;
a terminal cast is not proof that the runtime state matches the advertised type.

Adapted from [Matteo Collina's generic patterns](https://github.com/mcollina/skills/tree/main/skills/typescript-magician)
and [Seth Hobson's advanced types](https://github.com/wshobson/agents/tree/main/plugins/javascript-typescript/skills/typescript-advanced-types).
Matt Pocock explains [`NoInfer`](https://www.totaltypescript.com/noinfer) and
[legitimate type-level `any`](https://www.totaltypescript.com/any-considered-harmful).
