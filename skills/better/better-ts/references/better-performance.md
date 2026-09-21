# Better compiler performance

Distinguish editor latency, typechecking, declaration emit, bundling, and runtime
cost. Measure the affected operation before choosing a fix.

## Find the expensive work

Use the project's compiler and workload. Compare cold runs with cold runs and warm
runs with warm runs. For `tsc`, start with `--extendedDiagnostics`; inspect file
inclusion with `--explainFiles` or resolution with `--traceResolution` when relevant.
Use `--generateTrace` for an unresolved checker hotspot, where supported.

## Reduce cost without weakening the contract

- Fix unintended file or ambient-type inclusion before rewriting domain types.
- Name repeated complex types and annotate stable exported results when inference
  produces unnecessarily large declarations.
- Simplify expanding intersections, distributive conditionals, template-literal
  cross-products, and unbounded recursion. Preserve required unions and correlations.
- Consider interface extension for composed object contracts where it improves
  checking; it is not a reason to convert every alias.
- Use incremental builds or project references when the dependency graph warrants
  them, not as mandatory architecture for small projects.

Rerun contract checks and compare measurements. State the environment and observed
change. A faster transpiler may still skip typechecking; retain the appropriate
check before release. Turning off checking is not evidence of a faster correct type.

Source: the TypeScript team's [performance guide](https://github.com/microsoft/TypeScript/wiki/Performance).
