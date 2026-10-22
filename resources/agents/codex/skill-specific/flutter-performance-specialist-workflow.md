---
name: flutter-performance-specialist-workflow
description: Route, audit, plan, implement, and validate evidence-based Flutter performance work.
---
# Flutter Performance Specialist Workflow

Handle normal performance requests directly. Read `AGENTS.md`, the router skill, and the relevant focused skills. Inspect the app and available measurements before recommending or changing code.

Treat the user's description of their own system as the working fact. Check the real files or output, report facts only, and if the evidence differs say so once, neutrally, with the file and line, then carry on with their description. Do not contradict first and concede later.

## Modes

### Audit and plan

Use `flutter-perf-audit` to map the architecture and classify evidence-backed findings. Do not assume a hotspot from widget shape or user perception. When the user asks for a plan only, use the optional `flutter-performance-specialist-implementation-planner` helper when available, review its evidence, and save the plan in the required format. Do not edit implementation files in plan-only mode.

### Implement

Implement only findings supported by the audit and approved by the user. Route each concern to its focused `flutter-perf-*` skill. Make one targeted change at a time. Preserve calculations, business behaviour, access, API contracts, navigation, and visual design.

### Validate and review

Use `flutter-perf-profiling-and-validation` for performance claims and relevant regression checks. Use profile mode and realistic data for performance measurements. Review the diff against the skills and target project's conventions. Report checks not run and evidence gaps.

## Hard rules

- Do not claim an optimisation without measurements.
- Do not add dependencies, caches, or schema changes without demonstrated need.
- Do not run CPU work in an isolate unless profiling shows it blocks the UI thread.
- Never describe an app as fully optimised.
