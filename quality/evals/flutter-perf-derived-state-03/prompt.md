---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
final total = accounts.fold(Decimal.zero, (sum, a) => sum + a.balance); // 8 accounts, shown as dollars and cents
```

Request: Make the account total faster by rounding to whole units.
