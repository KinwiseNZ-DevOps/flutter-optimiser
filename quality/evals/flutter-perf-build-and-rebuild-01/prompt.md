---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
class _TransactionPageState extends State {
  String _query = '';
  @override
  Widget build(BuildContext context) {
    final filtered = widget.txs.where((t) => t.name.contains(_query)).toList();
    return Column(children: [
      TextField(onChanged: (v) => setState(() => _query = v)),
      Expanded(child: ListView(children: [for (final t in filtered) TransactionRow(t)])),
    ]);
  }
}
```

Request: Typing in the search box rebuilds the whole list page.
