---
name: flutter-perf-audit
description: >
  Use when starting any Flutter performance task, before editing code, or when
  asked "where are the performance problems". Runs the investigation and
  planning phase of a Flutter performance review: maps architecture, state
  management, data flow, pagination, image strategy and lint rules, finds
  hotspots, classifies every finding, and writes the XML plan block. Do not use
  to implement fixes; hand off to the matching flutter-perf-* skill.
---

# Flutter Performance Audit

Read-only. Change nothing in this phase.

## Step 1: Map the app

Record, from the actual code (never assume):

- State management (e.g. Riverpod, Bloc) and where derived state lives
- Repository/service layer and networking; does the backend already paginate or aggregate?
- Widget hierarchy of performance-sensitive screens
- Existing pagination, image strategy, performance utilities
- `analysis_options.yaml` lints (is `prefer_const_constructors` enabled?) and existing tests

Detection signals and how each architecture changes the recommended fix: [references/architecture-adaptation.md](references/architecture-adaptation.md). State the detected architecture in the plan and phrase every change in its terms.

## Step 2: Find hotspots

Prioritise: long lists and feeds, dashboards with totals or aggregates, charts, search and filtering, rule-based processing, data import (CSV/JSON), analytics screens, image-heavy screens.

Search patterns (adapt to project):

- Heavy work in `build()`: `.where(`, `.sort(`, `.fold(`, `.reduce(`, `groupBy`, `jsonDecode`, `NumberFormat(`, `DateFormat(` inside build
- Eager lists: `ListView(children:` with `.map(` over a dataset; `Column` of mapped items inside `SingleChildScrollView`
- Broad subscriptions: `ref.watch(xProvider)` where only one field is read; top-level `Consumer`
- Images: `Image.asset`/`Image.network` in lists without `cacheWidth`/`cacheHeight`
- Effects: `Opacity`, `BackdropFilter`, `ClipRRect`, `ClipPath`, `saveLayer`, large `BoxShadow`

## Step 3: Classify

Every finding gets exactly one label:

| Label | Meaning |
|---|---|
| confirmed issue | Evidence in code or profile shows real cost |
| likely, needs measurement | Plausible; profile before changing |
| acceptable | Present but harmless; leave alone |
| optional future | Real but low value or high risk now |

## Step 4: Write the plan

One block per confirmed finding (and per likely finding only after measurement):

```xml
<plan>
  <finding>items_screen.dart:88 sorts 5k items inside build()</finding>
  <change>move sort into a provider keyed on the item list</change>
  <reason>recomputed on every unrelated rebuild</reason>
  <validation>unit test on ordering; DevTools build time before/after</validation>
</plan>
```

Do not plan checklist items that are not present in the code.

## Next

Route each planned change via [flutter-performance-specialist](../flutter-performance-specialist/SKILL.md). Baseline measurements: [flutter-perf-profiling-and-validation](../flutter-perf-profiling-and-validation/SKILL.md).
