---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
ListView(
  children: [for (final row in history) HistoryRow(row)], // history has about 2000 rows
)
```

Request: The history ListView builds all rows at once.
