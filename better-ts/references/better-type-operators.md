# Better type operators

Start with built-in utilities. Write a custom transformation only when its input
domain, output relationship, and edge cases are clear. A smaller contract is often
more useful than a general-purpose type-level programming language.

## Conditional types and distribution

A conditional with a naked type parameter distributes over union members.
Wrap the checked type in a tuple when the question is about the whole union.

```ts
type EachArray<T> = T extends unknown ? T[] : never;
type WholeArray<T> = [T] extends [unknown] ? T[] : never;
type IsNever<T> = [T] extends [never] ? true : false;

type Separate = EachArray<string | number>; // string[] | number[]
type Mixed = WholeArray<string | number>; // (string | number)[]
type Empty = EachArray<never>; // never
type EmptyArray = WholeArray<never>; // never[]
```

`extends` tests assignability, not exact equality. `never` has no members to
distribute over, while `any` can affect both branches. Test these explicitly when
they are in the supported input domain. Do not add a tuple wrapper merely to make
an error disappear; it changes the meaning of the transformation.

Use `infer` to extract a relationship instead of requiring callers to restate it:

```ts
type ElementOf<T> = T extends readonly (infer Item)[] ? Item : never;
type RouteParam<S extends string> =
  S extends `${string}/:${infer Param}` ? Param : never;

type Mode = ElementOf<readonly ["read", "write"]>;
type Param = RouteParam<"/users/:userId">;
```

The route example only extracts a final placeholder from this simple shape. It is
not a URL parser or runtime validator. Use a router's existing types for its actual
grammar, or document and test a deliberately limited custom grammar.

## Mapped types and modifiers

```ts
type Handlers<T> = {
  [K in keyof T as K extends string ? `on${Capitalize<K>}` : never]:
    (value: T[K]) => void;
};

type FieldHandlers = Handlers<{ readonly title: string; count?: number }>;
```

Mapping over `keyof T` can preserve optional and readonly modifiers, including this
key-remapping pattern. Adding `+?` makes every resulting property optional; it does
not mean "preserve optionality." Use `-?` or `-readonly` only when changing that
contract is intended. Decide how string, numeric, and symbol keys should behave.

Check whether a transformation should operate on each member of an object union.
`keyof` a union exposes keys safe across its members, and `Pick`/`Omit` may therefore
lose variant-specific detail. A distributive wrapper is useful only when memberwise
behavior is the required contract. Likewise, `Partial<T>` is shallow; it is not a
complete description of a nested patch protocol.

## Bound recursion and string expansion

Before writing a deep utility, define behavior for tuples versus arrays, optional
properties, functions, dates, maps, sets, unions, and recursive structures that the
project actually uses. Treat unsupported shapes as unsupported, rather than silently
stripping their behavior. Deep readonly types still do not freeze runtime values.

Template literal unions can multiply into a large cross-product. Prefer an existing
schema, an explicit finite set, or generation when enumerating every possible string
becomes expensive. For recursive types, choose a stopping rule and a truthful result
at that limit. `any` as a recursion escape hatch silently removes the guarantee.

An object intersection means both contracts hold. It does not model object-spread
overwrite semantics for colliding keys. Even `Omit<A, keyof B> & B` needs care when
the overriding fields are optional or the inputs are unions. Keep such helpers
scoped to the runtime operation they actually describe.

Verify positive and rejected uses with [better-verification](better-verification.md).
Measure expensive transforms with [better-performance](better-performance.md).

Adapted from [Matteo Collina's type operators](https://github.com/mcollina/skills/tree/main/skills/typescript-magician)
and [Seth Hobson's advanced types](https://github.com/wshobson/agents/tree/main/plugins/javascript-typescript/skills/typescript-advanced-types),
checked against the TypeScript handbook on [conditional](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)
and [mapped types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html).
