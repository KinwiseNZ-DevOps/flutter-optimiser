# Flutter Performance Skills

A set of agent skills for auditing and optimising any production Flutter app, based on Flutter's official performance guidance. One router skill plus eight focused skills following the [Agent Skills](https://agentskills.io/specification) format.

Principle: inspect first, measure, change only what evidence justifies, preserve all business behaviour.

## Skills

<!-- catalogue:begin -->
| Skill | Purpose |
|---|---|
| [`flutter-perf-audit`](skills/flutter-perf-audit/SKILL.md) | Runs the investigation and planning phase of a Flutter performance review: maps architecture, state management, data flow, pagination, image strategy and lint rules, finds hotspots, classifies every finding, and writes the XML plan block. |
| [`flutter-perf-build-and-rebuild`](skills/flutter-perf-build-and-rebuild/SKILL.md) | Fixes expensive work inside Flutter build() methods and over-broad rebuilds: repeated sorting/filtering/formatting in build, widget-returning helper methods, missing const, builder widgets rebuilding static children, and state subscriptions (Riverpod select, Consumer, scoped providers) that rebuild too much. |
| [`flutter-perf-derived-state`](skills/flutter-perf-derived-state/SKILL.md) | Optimises derived and aggregated state in a Flutter app without changing results: totals, counts, percentages, balances, scores, grouped or sorted collections, and other values computed from large datasets. |
| [`flutter-perf-images-and-assets`](skills/flutter-perf-images-and-assets/SKILL.md) | Audits and fixes Flutter image and asset performance: oversized source images, images decoded far larger than displayed, cacheWidth/cacheHeight, resolution-aware asset variants (arbitrary densities, not just 1x/2x/3x), duplicate density variants, and choosing SVG/vector vs raster for logos, icons, illustrations, photos and thumbnails. |
| [`flutter-perf-isolates`](skills/flutter-perf-isolates/SKILL.md) | Decides whether and how to move CPU-heavy Flutter work off the main isolate using Isolate.run or compute: large CSV parsing, bulk categorisation, pattern or recurrence detection, big JSON parsing, image manipulation, large local analytics. |
| [`flutter-perf-lists-and-scrolling`](skills/flutter-perf-lists-and-scrolling/SKILL.md) | Improves scrolling and list performance in Flutter: replaces eager ListView(children: data.map(...).toList()) with ListView.builder, GridView.builder or sliver builders for large item histories, and evaluates itemExtent, prototypeItem and itemExtentBuilder to cut layout work. |
| [`flutter-perf-profiling-and-validation`](skills/flutter-perf-profiling-and-validation/SKILL.md) | Measures and validates Flutter performance work: flutter run --profile on physical devices, DevTools frame timings (UI vs raster), rebuild tracking, memory and oversized-image checks, realistic large datasets, flutter analyze and flutter test, calculation regression comparison, and backend migration checks when models change. |
| [`flutter-perf-rendering-effects`](skills/flutter-perf-rendering-effects/SKILL.md) | Investigates expensive Flutter visual effects and raster cost: Opacity, BackdropFilter, blur, clipping (ClipRRect, ClipPath), large or layered shadows, overlapping transparency, saveLayer, and continuously animated effects. |
| [`flutter-performance-specialist`](skills/flutter-performance-specialist/SKILL.md) | Orchestrates evidence-based performance audits and targeted optimisations of a production Flutter app. |
<!-- catalogue:end -->

## How they fit together

```
flutter-performance-specialist (router, guardrails, report format)
 ├─ flutter-perf-audit                    investigate + plan
 ├─ flutter-perf-build-and-rebuild        build(), helpers, const, rebuild scope
 ├─ flutter-perf-lists-and-scrolling      lazy lists, extents
 ├─ flutter-perf-derived-state  totals, caching, client vs backend
 ├─ flutter-perf-isolates                 CPU-bound work off the UI isolate
 ├─ flutter-perf-images-and-assets        decoding, densities, SVG vs raster
 ├─ flutter-perf-rendering-effects        blur, opacity, clips, shadows
 └─ flutter-perf-profiling-and-validation measure + verify + regression
```

## Layout

```
skills/<name>/SKILL.md      one folder per skill (+ references/ where needed)
templates/skill-template/   authoring template
evals/cases.jsonl           3+ behavioural cases per skill
scripts/skills_tool.py      validate, regenerate catalogue (stdlib only)
.github/workflows/          CI runs `skills_tool.py check`
AGENTS.md                   agent-facing repo map
```

## Install

Copy or symlink the `skills/` subfolders you want into your agent's skills directory (for Claude Code: `~/.claude/skills/` or a project's `.claude/skills/`). Install `flutter-performance-specialist` plus every `flutter-perf-*` skill; the router links to its siblings by relative path.

## Validate

```bash
python scripts/skills_tool.py check
```

## Status and limits

- Evals in `evals/cases.jsonl` are case definitions with expected behaviours; no runner or baseline results exist yet.
- Licensed under MIT (see [LICENSE](LICENSE)).
- Skills are based on Flutter's official docs and have not yet been tested against a live codebase.
