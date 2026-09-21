# Dependency injection, services, and layers

Use a service for a capability that needs a replaceable implementation or an
owned resource. Keep pure domain functions ordinary functions.
Give service identifiers a project/package namespace. The v4 class pattern shown here
is `Context.Service<Self, Shape>()("namespace/Name")`; construct layers explicitly.
Do not assume a service declaration automatically creates a `.Default` layer.

Here `GreetingLive` requires `Prefix`; `AppLive` supplies it and exposes `Greeting`:

```ts
import { Context, Effect, Layer } from "effect"

export class Prefix extends Context.Service<Prefix, {
  readonly value: string
}>()("example/Prefix") {}

export class Greeting extends Context.Service<Greeting, {
  readonly hello: (name: string) => Effect.Effect<string>
}>()("example/Greeting") {}

export const GreetingLive = Layer.effect(Greeting, Effect.gen(function*() {
  const prefix = yield* Prefix
  return Greeting.of({ hello: (name) => Effect.succeed(`${prefix.value} ${name}`) })
}))

export const AppLive = GreetingLive.pipe(
  Layer.provide(Layer.succeed(Prefix, Prefix.of({ value: "Hello" })))
)
```

Read each layer as `Layer<outputs, construction errors, inputs>`. For a layer A
requiring B, `A.pipe(Layer.provide(BLive))` wires B into A and exports A's outputs.
Use `provideMerge` when callers also need B's outputs. Merging sibling layers
combines outputs but does not automatically wire one into the other's inputs.

`Layer.effect` runs acquisition in the layer's scope and removes that Scope
requirement from the layer's inputs. Register resources there with
`Effect.acquireRelease`. Wrapping acquisition in `Effect.scoped` would close a
nested scope before callers use the returned service.

For missing `R`, trace the exact requirement from the failing operation through
construction and provisioning. Decide whether it belongs in the construction
layer or intentionally remains on a method (for example, a request-specific
capability). Do not erase requirements with casts or add `provideMerge` blindly.

Layer sharing depends on identity and the memoization context. Reuse a layer value
when sharing is intended. `Layer.fresh` isolates a layer's build; the `local`
option on `Effect.provide` builds for that provision instead of sharing across
provide calls. Check the installed signatures when using either. A factory that repeatedly constructs
a layer can duplicate acquisition. Check acquisition counts and release behavior
when changing graph lifetime. Test layers should satisfy the real service shape.

Read configuration during construction with `Config`; replace `ConfigProvider`
in tests. Keep secrets `Redacted` and avoid unwrapping them into logs.
`Context.Reference` fits a truthful default, not a hidden fallback for required
credentials or persistence. Construct dynamic layers with release-matched APIs
such as `Layer.unwrap` only when configuration actually changes the graph.

Distinguish missing configuration from invalid configuration. Apply defaults only
where the domain permits them; a malformed supplied URL or credential should not
silently select a development fallback. Define provider precedence at the
composition root. Test required, missing, invalid, and overridden values without
mutating process-global environment variables between concurrent tests.

Lookup: `src/Context.ts`, `src/Layer.ts`, `src/Config.ts`, `src/ConfigProvider.ts`.
