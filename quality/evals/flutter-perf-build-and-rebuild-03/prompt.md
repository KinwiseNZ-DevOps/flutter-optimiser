---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
class BalanceScreen extends ConsumerWidget {
  Widget build(BuildContext context, WidgetRef ref) {
    final balance = ref.watch(balanceProvider);
    return Column(children: [
      AnimatedBalanceHeader(balance: balance),
      const SummaryCard(title: 'This month'),
    ]);
  }
}
```

Request: Use a Consumer for an animated balance header with a static card below.
