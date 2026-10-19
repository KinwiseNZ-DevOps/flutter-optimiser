---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

Existing code in lib/features/transactions/transaction_page.dart:
```dart
class TransactionPage extends StatelessWidget {
  const TransactionPage({super.key, required this.txs});
  final List txs;

  Widget _buildHeader() => const Text('Transactions', style: TextStyle(fontSize: 20));
  Widget _buildRow(Tx tx) => ListTile(title: Text(tx.name), trailing: Text(tx.amountLabel));
  Widget _buildFooter() => const Divider();

  @override
  Widget build(BuildContext context) => Column(children: [
        _buildHeader(),
        Expanded(child: ListView(children: [for (final tx in txs) _buildRow(tx)])),
        _buildFooter(),
      ]);
}
```

Request: Replace every method that returns a Widget with a StatelessWidget for performance.
