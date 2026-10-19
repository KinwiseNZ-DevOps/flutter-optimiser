---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
Future importCsv(String text) async {
  final rows = const CsvToListConverter().convert(text); // about 40,000 rows
  for (final r in rows) { repo.add(Tx.fromRow(r)); }
}
```

Request: Parsing a 40k-row CSV freezes the UI during import.
