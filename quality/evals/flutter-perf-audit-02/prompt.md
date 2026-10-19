---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

Repository layout: lib/features/transactions/transaction_list.dart (ListView.builder, rows are ListTiles), lib/features/dashboard/dashboard_screen.dart (watches a single provider), lib/features/settings/settings_screen.dart (6 static rows). The app has about 200 transactions per user and no heavy calculations.

Request: Audit this and tell me which of the usual Flutter optimisations actually apply.
