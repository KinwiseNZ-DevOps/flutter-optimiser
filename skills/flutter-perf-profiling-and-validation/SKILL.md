---
name: flutter-perf-profiling-and-validation
description: >
  Measures and validates Flutter performance work: flutter run --profile on
  physical devices, DevTools frame timings (UI vs raster), rebuild tracking,
  memory and oversized-image checks, realistic large datasets, flutter analyze
  and flutter test, calculation regression comparison, and backend migration checks
  when models change. Use to capture a baseline before optimising, to confirm
  an improvement after, or whenever a performance claim needs evidence. Never
  judge performance from debug mode.
---

# Profiling and Validation

## 1. Measure in the right mode

```bash
flutter run --profile
```

- Use a representative physical Android and/or iOS device. Debug mode and emulators are not evidence of production performance.
- Open DevTools and inspect: frame timings, janky frames, UI work, raster work, rebuild behaviour, layout activity, memory, oversized images (Inspector).
- Judge frame budget against the device's actual refresh rate, not a fixed 16 ms.

If no physical device or profile run is possible, say so explicitly in `<remaining_issues>`; do not guess numbers.

## 2. Realistic data

- Validate with increasing sizes: hundreds, then several thousand records, and enough history to show scaling.
- Use an existing representative fixture if the project has one. Otherwise create a fixture, label it as synthetic, and keep it clearly separate from production data. Do not invent "production-scale" figures.

## 3. Baseline then compare

1. Record the baseline for the specific flow (screen, scroll, import) before the change.
2. Make one change.
3. Re-measure the same flow on the same device and data.
4. Report only measured values. No invented percentages.

## 4. Static and automated checks (after each meaningful change)

```bash
flutter analyze
flutter test
```

Run any additional project-specific checks found during the audit. No new warnings attributable to the work. Add or update tests where behaviour is touched.

## 5. Behaviour checks

- Inspect the affected UI: it looks and behaves the same.
- **Calculation regression:** where calculation structure changed, run old vs new on identical source data and compare exactly. See [flutter-perf-derived-state](../flutter-perf-derived-state/SKILL.md).
- **State boundaries:** required widgets still update, unrelated ones do not, nothing is stale, navigation lifecycle correct.
- **Lists:** scroll a large representative dataset.
- **Images:** Inspector oversized-image diagnostics where supported.

## 6. Backend migrations (only if models changed, after explicit approval)

Run the backend's own migration generation and apply commands.

The migration must be intentional, reviewable, and contain no unrelated schema changes. If no model change is needed, do not create migrations.

## 7. Report

Fill the sections in [report-format](../flutter-performance-specialist/references/report-format.md). State exactly what was investigated, changed, measured, verified. Never claim "fully optimised".
