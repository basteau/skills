# Better configuration

Configuration describes how this project is checked, built, and executed. Inspect
the installed compiler, package scripts, inherited configs, package metadata, and
actual runtime before changing a flag. Follow framework-maintained defaults where
they implement the project's build contract.

## Match the execution model

| Execution path | Check |
| --- | --- |
| JavaScript emitted by `tsc` and run in Node | Compatible Node module/resolution settings, package `type`, emitted extensions, runtime version |
| Code transformed by a bundler | Its supported resolution and module settings; a separate typecheck if transformation skips checking |
| TypeScript executed by a runtime or loader | That runtime's syntax, extension, alias, and type-stripping support |
| A published package | Emitted declarations and export paths work in the consumers the package supports |

Bundler resolution can accept imports that plain Node will reject. Compiler `paths`
does not itself rewrite runtime imports. `target` controls emitted syntax, while
`lib` supplies declarations; neither installs missing runtime APIs or polyfills.
Include DOM or server globals only in the environments where they exist.

Node's built-in type stripping is not a typecheck and does not read `tsconfig.json`
to implement path aliases or downlevel JavaScript. Verify supported syntax and
extensions against the project's Node version. When that execution mode is chosen,
consider compiler checks such as `erasableSyntaxOnly` where supported. A successful
development loader run does not prove the emitted package runs in production.

Use `import type` and `export type` for type-only dependencies. Preserve required
side-effect imports explicitly. `verbatimModuleSyntax` can make this separation
visible to the checker, but changing it may expose an ESM/CJS mismatch that needs
an actual module decision rather than a suppression.

## Choose strictness deliberately

Prefer `strict` for new code. For existing projects, treat stricter flags as a
scoped migration: understand the new errors and their behavioral implications.
Do not disable a check to make a local implementation pass.

| Option | What it exposes |
| --- | --- |
| `strictNullChecks` (within `strict`) | Missing values must be represented and narrowed |
| `noUncheckedIndexedAccess` | Unproven array and dictionary reads may be absent |
| `exactOptionalPropertyTypes` | Omission differs from explicitly assigning `undefined` |
| `noImplicitOverride` | Class overrides must remain intentional |

The latter three are separate from `strict`. With exact optional properties,
`field?: T` still reads as potentially `undefined`, but explicitly writing
`undefined` requires that value in the property's allowed type. Match patch,
serialization, and clearing semantics before migrating.

Keep declaration checking and project checking distinct. `skipLibCheck` can be an
existing performance tradeoff; it is not a fix for incorrect application code or
proof that conflicting dependency declarations are compatible.

## Verify consumers and migrations

For packages, inspect emitted `.d.ts` files, package exports, and declaration paths.
Check the packed artifact with representative supported consumers; workspace aliases
and development dependencies can conceal missing exports or leaked internal types.
Do not claim dual ESM/CJS support without exercising both entry points.

For JavaScript migration, use existing `allowJs`, `checkJs`, or JSDoc support to move
one useful boundary at a time. Keep interop adapters small and preserve runtime
behavior. Avoid mass-renaming files followed by broad `any` or `@ts-ignore` patches.

Sources: Matt Pocock's [configuration guidance](https://www.totaltypescript.com/tsconfig-cheat-sheet),
TypeScript's [module model](https://www.typescriptlang.org/docs/handbook/modules/theory.html),
[`noUncheckedIndexedAccess`](https://www.typescriptlang.org/tsconfig/noUncheckedIndexedAccess.html),
[`exactOptionalPropertyTypes`](https://www.typescriptlang.org/tsconfig/exactOptionalPropertyTypes.html),
and Node's [TypeScript execution documentation](https://nodejs.org/api/typescript.html).
