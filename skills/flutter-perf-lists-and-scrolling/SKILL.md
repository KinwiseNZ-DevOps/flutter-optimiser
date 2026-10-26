---
name: flutter-perf-lists-and-scrolling
description: >
  Use for janky scroll, slow first paint of long lists, or thousands of widgets
  built at once. Improves scrolling and list performance in Flutter: replaces
  eager ListView(children: data.map(...).toList()) with ListView.builder,
  GridView.builder or sliver builders for large item histories, and evaluates
  itemExtent, prototypeItem and itemExtentBuilder to cut layout work. Do not use
  for small fixed lists, and not for per-item rebuild scope (flutter-perf-build-
  and-rebuild).
---

# Lists and Scrolling

## Decide whether to act

| Situation | Action |
|---|---|
| Dataset can grow large or is dynamic (histories, feeds, search results, activity logs) | Use lazy construction |
| Small fixed UI list (settings rows, 5 tabs) | Leave it |
| Already `.builder` | Check extents and item cost instead |

## Eager pattern to remove

```dart
ListView(children: items.map((item) => ItemTile(item)).toList())
```

This builds every item up front. Replace with:

```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, i) => ItemTile(items[i]),
)
```

Equivalent lazy APIs: `GridView.builder`, `SliverList.builder`, `SliverGrid.builder` inside `CustomScrollView`. A `Column` of mapped items inside `SingleChildScrollView` is the same eager problem.

## Known item sizes

If items share a predictable main-axis size, assess:

- `itemExtent`: fixed extent for every item
- `prototypeItem`: sizes all items like a sample widget
- `itemExtentBuilder`: per-index extent when sizes vary but are computable

These let the sliver skip measuring each child. Do not apply when item height depends on content (wrapping text) unless it can be computed exactly; a wrong extent clips or misaligns content.

## Item cost

- Keep `itemBuilder` cheap; no sorting, formatting setup, or aggregation inside it. Prepare item view-models upstream.
- Use stable keys when items reorder or are removed, so state follows the right row.
- Group headers (by date) belong in prepared data or a sliver structure, not recomputed per frame.
- Images in rows: see [flutter-perf-images-and-assets](../flutter-perf-images-and-assets/SKILL.md).

## Very large data

If the client receives thousands of records only to show a page, check for existing backend pagination before adding client tricks. See [flutter-perf-derived-state](../flutter-perf-derived-state/SKILL.md) section "Client vs backend".

## Verify

Profile-mode scroll through a representative large list (hundreds to several thousand rows); compare jank frames before/after. See [flutter-perf-profiling-and-validation](../flutter-perf-profiling-and-validation/SKILL.md).
