---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

The app uses flutter_bloc, not Riverpod.
```dart
// BlocBuilder of AppBloc and AppState
BlocBuilder(
  builder: (context, state) => Column(children: [Header(state.user), BalanceCard(state.balance), TxList(state.txs)]),
)
```

Request: This app uses Bloc, not Riverpod. Find over-broad rebuilds.
