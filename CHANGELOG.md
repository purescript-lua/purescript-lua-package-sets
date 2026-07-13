# Changelog

Notable changes to the purescript-lua package set are recorded here. Each entry
is a `psc-*` set release: the combination of fork versions consumers pin through
`workspace.packageSet.url`. The format is based on
[Keep a Changelog][keepachangelog], and entries are assembled from fragments in
`changelog.d/` with [scriv][scriv] on each release ([ADR 0009][adr0009]).

`scriv` for this repository comes from the pslua dev shell
(`nix develop github:purescript-lua/purescript-lua`), since the set has no flake
of its own.

<!-- scriv-insert-here -->

<a id='changelog-psc-0.15.15-20260713'></a>
## psc-0.15.15-20260713 - 2026-07-13

### Changed

- arrays v7.4.2: `Data.Array.ST` FFI restored to the STFn convention —
  `thaw`, `freeze`, `withArray`, `toAssocArray` no longer crash on a nil
  foreign, and `peek` returns the `Maybe` instead of a thunk
  (purescript-lua/purescript-lua#186).
- numbers v9.1.5: `fromStringImpl` is 4-ary as its `Fn4` declaration
  requires, so `Data.Number.fromString` returns a `Maybe Number` instead of
  a closure (purescript-lua/purescript-lua#186).
- integers v6.1.3: `toStringAs` accumulates digits by appending and
  reverses once, replacing the quadratic front-insertion
  (purescript-lua/purescript-lua#186).
- lua-ngx v0.3.0: `say` and `Http.Status.set` are backed by `EffectFn1`
  foreigns, so saturated call sites compile to a single direct Lua call
  (purescript-lua/purescript-lua#186).

<a id='changelog-psc-0.15.15-20260712-2'></a>
## psc-0.15.15-20260712-2 - 2026-07-12

### Changed

- prelude v7.3.1: `Data.Unit.unit` is pinned to the shared foreign singleton
  (`@inline unit never`) so the table constructor is not duplicated per use
  site (purescript-lua/purescript-lua#176).

<a id='changelog-psc-0.15.15-20260712'></a>
## psc-0.15.15-20260712 - 2026-07-12

### Changed

- numbers v9.1.4: `Data.Number.isNaN` links (the FFI key was spelled `isNan`)
  and detects NaN via IEEE self-inequality instead of comparing `tostring`
  spellings (purescript-lua/purescript-lua-numbers#8).

## psc-0.15.15-20260624 - 2026-06-24

### Changed

- The set is published as a single consumable `packages.json` for the new spago:
  a RemotePackageSet (registry baseline with the Lua forks overlaid as git
  entries) that consumers point `workspace.packageSet.url` at. `src/packages.json`
  is now the source of truth ([ADR 0008][adr0008]).

### Removed

- The legacy `packages.dhall` release asset and the Dhall `upstream // lua` merge
  model. spago 0.21 / Dhall is no longer supported.

## psc-0.15.15-20260615-2 - 2026-06-15

### Fixed

- enums v6.1.2: `toCharCode` is length-guarded so a lone code-unit byte no longer
  crashes (#102).

## psc-0.15.15-20260615 - 2026-06-15

### Fixed

- The bulk of the FFI campaign landed here: console v6.1.1 (`error`/`warn` to
  stderr, #76 #77), numbers v9.1.2 and v9.1.3 (the `Number` formatting contract,
  #92–#98), integers v6.1.2 (32-bit `Int` semantics, #85–#91), enums v6.1.1
  (UTF-8 `Char` code points, #79 #80), effect v4.1.3 (`forE` half-open range,
  #78), exceptions v6.1.1 (Lua 5.1 `Effect.Exception`, #81–#84), arrays v7.4.0 and
  v7.4.1 (5.1-safe `table.pack`/`unpack`/`move`), st v6.4.0 (`ST.for` half-open
  range), prelude v7.3.0, and safe-coerce v2.0.1.

## psc-0.15.15-20260614-4 - 2026-06-14

### Fixed

- effect v4.1.2 and assert v6.1.1: luacheck-clean FFI.

## psc-0.15.15-20260614-3 - 2026-06-14

### Fixed

- control v6.0.1 (`arrayExtend` for Lua 5.1) and foldable-traversable v6.1.1
  (1-based `mapWithIndex`).

## psc-0.15.15-20260614-2 - 2026-06-14

### Fixed

- effect v4.1.1, integers v6.1.1, and numbers v9.1.1: the first batch of FFI
  bugfixes.

## psc-0.15.15-20260614 - 2026-06-14

### Fixed

- prelude v7.2.2: `Array` `Semigroup` append (`concatArray`).

## psc-0.15.15-20260613-2 - 2026-06-13

### Added

- strings v6.2.0: `Data.String.CodePoints` implemented for UTF-8.

## psc-0.15.15-20260613 - 2026-06-13

### Fixed

- prelude v7.2.1: Lua 5.1 FFI compatibility.

## psc-0.15.15-20260612 - 2026-06-12

### Fixed

- prelude v7.2.0 restored, bringing back `unit = {}` so `Array Unit` no longer
  collapses to an empty table ([ADR 0004][adr0004]).

<!-- scriv-end-here -->

## Earlier

Set releases before the 2026 FFI campaign (the `psc-0.15.8-*` and
`psc-0.15.15-2024*` series, 2023–2024) tracked upstream package-set updates and
the initial Lua forks. They predate this changelog and are recorded only as
`psc-*` tags.

[keepachangelog]: https://keepachangelog.com/en/1.1.0/
[scriv]: https://scriv.readthedocs.io/
[adr0004]: docs/adr/0004-unit-is-empty-table.md
[adr0008]: docs/adr/0008-new-spago-and-json-package-set.md
[adr0009]: docs/adr/0009-changelogs-via-scriv.md
