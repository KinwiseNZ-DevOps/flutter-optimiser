---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

lib/features/transactions/transaction_list.dart:
```dart
Widget build(BuildContext context, WidgetRef ref) {
  final all = ref.watch(transactionsProvider);
  final sorted = [...all]..sort((a, b) => b.date.compareTo(a.date));
  return ListView(children: [for (final t in sorted) TransactionRow(t)]);
}
```
lib/features/dashboard/dashboard_screen.dart:
```dart
Widget build(BuildContext context, WidgetRef ref) {
  final state = ref.watch(appStateProvider);
  return Column(children: [BalanceHeader(state), SpendChart(state), RecentList(state)]);
}
```

Request: Where are the hotspots in the list and dashboard screens?
