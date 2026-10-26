---
name: flutter-perf-rendering-effects
description: >
  Use when DevTools shows high raster time or janky raster frames. Optimises
  only where profiling or implementation evidence shows real cost, and keeps the
  visual design materially unchanged. Investigates expensive Flutter visual
  effects and raster cost: Opacity, BackdropFilter, blur, clipping (ClipRRect,
  ClipPath), large or layered shadows, overlapping transparency, saveLayer, and
  continuously animated effects. Do not use for UI-thread rebuild problems
  (flutter-perf-build-and-rebuild).
---

# Rendering Effects

These effects are not forbidden. Investigate, then act only on evidence.

## 1. Locate the cost

- Profile mode, DevTools performance view: is the janky time in **raster** rather than UI?
- Use the raster/layer diagnostics available in DevTools (e.g. save-layer and repaint tooling) to find which widgets trigger offscreen layers.
- Static scan for: `Opacity`, `BackdropFilter`, `ImageFiltered`, `ClipRRect`, `ClipPath`, large `blurRadius` in `BoxShadow`, transparency stacked over transparency, always-running animations.

If the raster thread is fine, leave the effect alone and record it under `<not_changed>`.

## 2. Targeted remedies (apply only to a confirmed hotspot)

| Cost | Lower-cost option to evaluate |
|---|---|
| `Opacity` over a subtree | Apply opacity to a single leaf, use `AnimatedOpacity`/`FadeTransition` for animation, or fold alpha into the colour (`Color.withValues(alpha:)`/`withOpacity`) on a plain box or text |
| `BackdropFilter` | Limit to the smallest area; avoid inside scrolling lists; avoid animating its sigma; consider a static blurred or tinted alternative only if a visual compromise is acceptable |
| Clipping | Avoid clipping where `borderRadius` on a `DecoratedBox`/`Container` gives the same result; use the cheapest `clipBehavior` that is visually correct |
| Large shadows | Reduce blur radius or layer count, or pre-render only if profiling justifies it |
| Looping animations | Stop when off-screen; scope repaint with `RepaintBoundary` where profiling shows a benefit |

`RepaintBoundary` is a tool, not a default: add it only where repaint tracing shows an isolated subtree repainting needlessly.

## 3. Design preservation

Visual design must remain materially unchanged. If a compromise is truly required (e.g. dropping blur on low-end devices), state it explicitly in the report as a visual trade-off and get it noted, never slip it in silently.

## 4. Verify

Compare raster frame times before/after on a representative physical device in profile mode, and a visual comparison of the affected screen. Method: [flutter-perf-profiling-and-validation](../flutter-perf-profiling-and-validation/SKILL.md).
