---
name: flutter-performance-specialist
description: >
  Orchestrates evidence-based performance audits and targeted optimisations of a
  production Flutter app.
  Use when the user asks to audit, profile, speed up, or optimise Flutter
  performance: jank, slow scrolling, excess rebuilds, large lists,
  memory, image decoding, CPU-heavy work, or "should this move to the backend".
  Routes to the focused flutter-perf-* skills and enforces shared guardrails and
  the final report format. Do not use for non-performance Flutter work, new
  feature development, or backend-only performance tuning.
---

# Flutter Performance Specialist (router)

Senior-engineer stance: inspect first, measure where possible, change only what evidence justifies, preserve all business behaviour.

## Workflow

Copy and track:

```
- [ ] 1. Audit (flutter-perf-audit): map architecture, classify findings
- [ ] 2. Plan: <plan> block per justified finding
- [ ] 3. Implement incrementally, one concern at a time (route table below)
- [ ] 4. Validate after each change (flutter-perf-profiling-and-validation)
- [ ] 5. Report using references/report-format.md
```

Do not skip step 1. Do not implement items the audit did not confirm.

## Route by concern

| Concern | Skill |
|---|---|
| Map the app, classify findings, write the plan | [flutter-perf-audit](../flutter-perf-audit/SKILL.md) |
| Work in `build()`, helper methods, `const`, builders, rebuild scope, Riverpod subscriptions | [flutter-perf-build-and-rebuild](../flutter-perf-build-and-rebuild/SKILL.md) |
| Long or dynamic lists, grids, item extents | [flutter-perf-lists-and-scrolling](../flutter-perf-lists-and-scrolling/SKILL.md) |
| Totals, aggregates, grouping, caching, client vs backend aggregation | [flutter-perf-derived-state](../flutter-perf-derived-state/SKILL.md) |
| CSV parsing, JSON, pattern detection, main-isolate blocking | [flutter-perf-isolates](../flutter-perf-isolates/SKILL.md) |
| Image size, decoding, resolution variants, SVG vs raster | [flutter-perf-images-and-assets](../flutter-perf-images-and-assets/SKILL.md) |
| `Opacity`, `BackdropFilter`, clipping, shadows, save layers | [flutter-perf-rendering-effects](../flutter-perf-rendering-effects/SKILL.md) |
| Profile mode, DevTools, datasets, calculation regression checks | [flutter-perf-profiling-and-validation](../flutter-perf-profiling-and-validation/SKILL.md) |

Several concerns usually apply. Handle them in this order of expected payoff: derived state and rebuild scope, lists, isolates, images, effects.

## Always-on rules

Full text and rationale: [references/guardrails.md](references/guardrails.md). Summary:

1. A rebuild is not a bug. A widget-returning method is not a bug. Prove cost first.
2. Never change calculation semantics, permissions, data visibility, API contracts, or navigation.
3. No blanket rewrites, no new state-management framework, no new dependency without need.
4. Isolates are for CPU work, never for ordinary network or async I/O.
5. No speculative caches without an invalidation story.
6. No schema changes. If one becomes necessary, stop and explain impact first.
7. Never claim "fully optimised". Report what was investigated, changed, measured, verified.

## References

- [references/guardrails.md](references/guardrails.md): constraints with mechanisms
- [references/report-format.md](references/report-format.md): required final report sections
- [references/official-sources.md](references/official-sources.md): primary Flutter/Dart docs
