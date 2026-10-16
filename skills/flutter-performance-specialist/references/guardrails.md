# Guardrails

Contents: Misconceptions · Behaviour preservation · Scope control · Backend and schema · Frame timing

## Misconceptions to avoid

| Belief | Reality | Action |
|---|---|---|
| Widget-returning helper methods are performance bugs | Not automatically. They rebuild with their parent; the cost is the lost rebuild boundary and `const` opportunity | Extract only when it narrows rebuilds, enables `const`, is reused in a list item, or clarifies state ownership |
| Rebuilding is wrong | Rebuilding widgets is normal in Flutter | Act only when a rebuild is expensive or over-broad |
| Every list needs `.builder` | Small static lists gain nothing | Refactor lists that can grow large or are dynamic |
| Use isolates for anything `async` | `async` I/O does not block the UI isolate | Isolate only genuinely CPU-bound work |
| Images need exactly 1x/2x/3x | Flutter supports arbitrary density variants and picks the closest | Do not add or enforce a 1x/2x/3x-only rule |
| Frame budget is always 16 ms | Depends on device refresh rate | Read budget from actual device/DevTools |

## Behaviour preservation

Optimisation must not change: calculations, permissions, data visibility, categorisation and business rules, navigation, user-facing functionality, persistence semantics, API contracts.

Exception: a defect directly tied to the performance issue may be fixed only after it is documented before correction.

## Scope control

- No blanket rewrite; touch only the audited hotspot.
- No state-management framework swap. Use the existing mechanism (selectors, scoped providers, consumers, small stateful widgets).
- No unrelated feature changes, no invented endpoints, files, architecture or business rules.
- No premature optimisation of tiny static widgets or datasets.
- No new dependencies unless there is no reasonable in-framework option.
- Visual design stays materially unchanged unless a compromise is explicitly required and stated.

## Backend and schema

- Move work to the backend only where architecture, performance, and ownership of the calculation justify it. Never automatically.
- Do not invent backend capabilities. Verify the endpoint or aggregation exists.
- No schema changes for this task. If genuinely necessary, stop and explain reason and impact before changing anything.
- If backend models are changed after approval, run the backend's migration commands and confirm the migration contains only the intended change. Do not create empty or meaningless migrations.

## Frame timing

Interpret jank against the device's real refresh characteristics (60, 90, 120 Hz) using DevTools, not a fixed 16 ms assumption. Never judge production performance from debug mode.
