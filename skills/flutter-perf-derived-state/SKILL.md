---
name: flutter-perf-derived-state
description: >
  Use when the same values are recomputed on every rebuild or the client
  repeatedly scans thousands of records. Optimises derived and aggregated state
  in a Flutter app without changing results: totals, counts, percentages,
  balances, scores, grouped or sorted collections, and other values computed
  from large datasets. Covers memoisation, provider-level caching, incremental
  calculation, and deciding whether aggregation or pagination belongs on a
  backend instead of the client. Do not use to change calculation rules.
---

# Derived and Aggregated State

**Hard rule: calculation semantics are frozen.** Rounding, currency/unit handling, sign conventions, period boundaries and inclusion rules must produce identical output. Mechanism: a "faster" result that differs slightly (e.g. one cent in a money total) silently breaks user trust and is hard to detect later.

## 1. Find repeated derivation

Typical targets: totals per category or period, remaining-versus-limit values, percentages, account or group sums, progress values, grouping and sorting of large lists, detection results (recurring items, duplicates).

Symptom: the same scan (`.where` then `.fold`, repeated per widget) runs on every unrelated rebuild.

## 2. Recompute only when inputs change

Least to most invasive:

1. Compute once per build into a local variable (same-build duplicates).
2. Derive in a provider/ViewModel/controller keyed on its inputs, so it recalculates only when the source data changes.
3. Single-pass aggregation: one loop producing all totals instead of many `.where` passes.
4. Incremental update (apply a delta for one added/edited/removed item) only if a full recompute is proven too slow and correctness is testable.
5. Memoisation: only with a defined key and invalidation (next section).

## 3. Cache safety

Before adding any cache, write down:

- **Key:** exactly which inputs determine the value (data version, date range, user/account/tenant scope, locale/currency, filters)
- **Invalidation:** which events clear it (create/update/delete, sync, import, scope change, rule change, time rollover)
- **Staleness risk:** what the user sees if invalidation is missed

If any of the three is unclear, do not cache. Scope and permissions must be part of the key; never share a cached value across scopes.

## 4. Client vs backend

The client should not behave like a database engine. Signs: very large datasets downloaded, then multiple `.where()` passes, sorting, grouping, reducing.

Move work to a backend only when all hold:

- an aggregation or paginated endpoint **already exists**, or adding one is within the agreed task (verify; never invent endpoints)
- ownership of the calculation and its rules can live server-side without behaviour change
- measurement or clear scaling evidence shows the client cost matters

No schema changes for this task. If an optimisation truly needs one, stop and explain reason and impact first. See [guardrails](../flutter-performance-specialist/references/guardrails.md).

## 5. Prove equivalence

Run old and new implementation on identical source data and compare outputs exactly (use decimal-safe comparison if the project avoids floating point for money or precision-sensitive values). Include edge cases: empty data, period boundaries, negative values, mixed units/currencies, scoped or permission-limited data. Keep this as an automated test.

## Hard rules

- Act only on what the user asked. Before answering or changing anything, open the relevant files and check what is already done; report only what you checked, never guess, and do not redo finished work. Reason: guesses and repeated work waste the user's credit.

## Next

Heavy one-off computations that still block frames: [flutter-perf-isolates](../flutter-perf-isolates/SKILL.md).
