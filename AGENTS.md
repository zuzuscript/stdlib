# ZuzuScript Standard Library

This repository contains the shared ZuzuScript standard library. All
ZuzuScript runtimes should bundle these modules and run the shared stdlib
tests where their host capabilities allow it.

Use Oxford English in documentation: mostly standard British English, with
`-ize` word endings.

## Relationship To Other Projects

`stdlib` is consumed as a submodule by `zuzu-perl`, `zuzu-rust`, `zuzu-js`,
and the matrix project. Runtime repositories provide native implementations
for runtime-supported modules, while this repository defines the shared
module surface, Pure Zuzu Modules, POD stubs, tests, fixtures, and helper
modules.

Do not rely on sibling checkouts. Runtime-specific fixes belong in the
runtime repositories; shared module fixes belong here.

## Project Shape

- `modules/std/` contains standard modules.
- `modules/test/` contains test helper modules.
- `tests/` contains stdlib ztests.
- `test-fixtures/` contains data used by stdlib tests.
- `scripts/zuzuzoo` supports distribution/package workflows.
- `modules/javascript.zzm` and `modules/perl.zzm` are capability modules for
  runtimes that support those host integrations.

## Module Rules

Prefer Pure Zuzu Modules whenever the functionality can be implemented in
ZuzuScript. Runtime-supported modules should be POD/module-surface stubs here
with native implementation in each runtime that supports the capability.

If a Pure Zuzu Module exists, runtimes must execute it through normal module
loading and evaluation. Do not add or request runtime shortcuts for Pure Zuzu
Modules, especially `std/path/*`, `std/path/z`, and `std/path/zz`.

`std/path/z` is the Pure Zuzu Module implementation of ZPath and should stay
modular enough for `std/path/zz` to share or extend its pieces.

## Tests

Stdlib tests are ztests and emit TAP. They are usually run from runtime
repositories or through the matrix, for example:

```bash
# from zuzu-perl
prove -lr t

# from zuzu-js
node run-ztests.js --include-std

# from zuzu-rust
cargo run --bin zuzu-rust-run-tests -- stdlib/tests
```

When changing a module, add or update focused tests under `tests/`. Avoid
changing fixtures unless the data itself is the subject of the change.

## ZuzuScript Style

Use tabs for indentation, spaces for alignment, One True Brace Style,
uncuddled `else`, whitespace around binary operators, and semicolons as
terminators. Keep code lines under 80 columns where practical.

POD documentation should use spaces in code samples, not tabs.
