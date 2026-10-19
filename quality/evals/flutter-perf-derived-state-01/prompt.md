---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
Widget build(BuildContext context, WidgetRef ref) {
  final txs = ref.watch(transactionsProvider);
  final totals = {}; // category name to amount
  for (final t in txs) { totals[t.category] = (totals[t.category] ?? 0) + t.amount; }
  return CategoryTotals(totals);
}
```

Request: Per-category totals are recomputed on every rebuild; cache them.
