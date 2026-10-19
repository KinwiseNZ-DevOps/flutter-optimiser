---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
ListView.builder(
  itemCount: notes.length,
  itemBuilder: (context, i) => ListTile(title: Text(notes[i].body, softWrap: true)), // 1 to 6 lines each
)
```

Request: Rows have wrapping text of variable height; add itemExtent to speed scrolling.
