---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
Widget build(BuildContext context, WidgetRef ref) {
  final records = ref.watch(recordsProvider); // about 3000 records
  final grouped = groupBy(records..sort((a, b) => b.date.compareTo(a.date)), (r) => r.monthKey);
  return MonthList(groups: grouped);
}
```

Request: build() sorts and groups 3000 records every time.
