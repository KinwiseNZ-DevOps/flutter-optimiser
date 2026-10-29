---
name: flutter-perf-build-and-rebuild
description: >
  Use when a screen rebuilds too much, a toggle or search input rebuilds a whole
  page, or build() does computation. Fixes expensive work inside Flutter build()
  methods and over-broad rebuilds: repeated sorting/filtering/formatting in
  build, widget-returning helper methods, missing const, builder widgets
  rebuilding static children, and state subscriptions (Riverpod select,
  Consumer, scoped providers) that rebuild too much. Do not use for list
  construction (flutter-perf-lists-and-scrolling) or derived-state caching
  (flutter-perf-derived-state).
---

# Build Methods and Rebuild Scope

A rebuild is normal. Act only when the rebuilt work is expensive or the scope is clearly too wide.

## 1. Work inside `build()`

Look for: sorting, filtering, grouping, aggregation, mapping large collections, repeated `.contains()` on big lists, repeated formatting, JSON transformation, business-rule processing.

- Same value computed several times in one build: compute once into a local variable.
- Derived value that only changes when inputs change: move to a provider/ViewModel/controller/repository so the UI renders prepared state. See [flutter-perf-derived-state](../flutter-perf-derived-state/SKILL.md) for calculated values.
- `build()` stays pure and cheap; no I/O, no state mutation.

## 2. Widget-returning helper methods

Do not replace them blindly. Extract to a `StatelessWidget`/`StatefulWidget` only when it gives one of:

- smaller rebuild boundary
- `const` opportunity
- reuse, or repeated list-item rendering
- clearer state ownership
- real maintainability gain

Skip extraction for one-off, cheap, non-repeated fragments. Avoid swarms of tiny classes with no benefit.

## 3. Rebuild scope

Typical over-broad triggers: visibility toggles, single item selection, search input, dropdown change, loading indicator, progress indicators, chart selection, one record or one user's state changing.

Narrow with the project's existing mechanism, in order of least invasiveness:

1. Move the changing state into a small `StatefulWidget` below the large subtree.
2. Riverpod: `ref.watch(provider.select((s) => s.field))` so only the used field triggers rebuilds.
3. Wrap only the dependent part in a `Consumer`; watch family/scoped providers per item.
4. Split the widget so the unchanged part is a `const` child.

Do not switch state-management framework. After narrowing, confirm required widgets still update, unrelated ones do not, state is not stale, and navigation lifecycle still works.

## 4. `const`

Make constructors/widgets `const` where it is safe and semantics do not change. Check whether `prefer_const_constructors` and `prefer_const_literals_to_create_immutables` are enabled in `analysis_options.yaml`; enabling them is the reliable way to find opportunities. Never alter behaviour just to force `const`.

## 5. Builders and static children

For `AnimatedBuilder`, `ValueListenableBuilder`, `ListenableBuilder`, Riverpod `Consumer`, and similar: pass subtrees that do not depend on the changing value through the builder's `child` parameter. Add no abstraction beyond this unless it measurably reduces work.

## Verify

Use DevTools rebuild counts ("Track widget rebuilds") in profile mode; see [flutter-perf-profiling-and-validation](../flutter-perf-profiling-and-validation/SKILL.md).

## Hard rules

- Act only on what the user asked. Before answering or changing anything, open the relevant files and check what is already done; report only what you checked, never guess, and do not redo finished work. Reason: guesses and repeated work waste the user's credit.
