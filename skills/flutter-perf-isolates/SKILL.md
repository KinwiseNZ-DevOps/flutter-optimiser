---
name: flutter-perf-isolates
description: >
  Use when DevTools shows UI-thread frames blocked by Dart computation. Decides
  whether and how to move CPU-heavy Flutter work off the main isolate using
  Isolate.run or compute: large CSV parsing, bulk categorisation, pattern or
  recurrence detection, big JSON parsing, image manipulation, large local
  analytics. Do not use for ordinary network or async I/O, or for small
  computations where isolate overhead exceeds the work.
---

# Isolates for CPU-Heavy Work

## Gate: is an isolate justified?

Answer all three before writing code:

1. **Is it CPU-bound Dart work?** `await http.get(...)` is I/O; it does not block the UI isolate. Never isolate it.
2. **Is there evidence it hurts frames?** Profile (DevTools, profile mode) and find the long UI-thread task. No evidence means mark "likely, needs measurement", not implement.
3. **Does the work outweigh overhead?** Isolate startup and copying data across the boundary cost time. Small inputs get slower.

If any answer is no, stop.

## Candidates

Large dataset aggregation, large CSV parsing, bulk categorisation, pattern or recurrence detection, large JSON parsing, image manipulation, heavy maths, large local analytics.

## Implementation

Prefer `Isolate.run()` for a one-shot task; `compute()` is acceptable where the project already uses it. Use a long-lived isolate only for repeated work with proven benefit.

```dart
final result = await Isolate.run(() => parseCsv(rawBytes));
```

Rules that prevent the common failures:

- The function passed must be a top-level or static function (or closure capturing only sendable values). Capturing a `BuildContext`, a Riverpod `ref`, or objects with platform handles fails or misbehaves.
- Pass plain data in, return plain data out. Keep the transferred payload small; send raw bytes or a compact structure rather than a large object graph.
- Platform channels and plugins are not available in a background isolate unless explicitly set up; do the plugin call on the main isolate, then hand the data over.
- Errors thrown inside propagate as the future's error; handle them where you await.
- Keep calculation logic identical to the main-isolate version. Share the same pure function so results cannot diverge.

## After moving

- Show progress/loading state; the UI is now free but the result is delayed.
- Handle cancellation or screen disposal so a stale result does not overwrite newer state.
- Re-measure. Report the real before/after, including any overhead on small inputs.
- Verify results match the original on identical data (calculation rules in [flutter-perf-derived-state](../flutter-perf-derived-state/SKILL.md)).

## Alternative

If the cost comes from repeated scans, redesign (single pass, caching, backend aggregation) before reaching for an isolate.

## Hard rules

- Act only on what the user asked. Before answering or changing anything, open the relevant files and check what is already done; report only what you checked, never guess, and do not redo finished work. Reason: guesses and repeated work waste the user's credit.
