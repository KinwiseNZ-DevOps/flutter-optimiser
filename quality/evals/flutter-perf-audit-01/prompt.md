---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

Repository layout: lib/features/dashboard/dashboard_screen.dart (a ConsumerWidget watching one big appStateProvider), lib/features/transactions/transaction_list.dart (a ListView with children built from a list), lib/shared/glass_card.dart (uses BackdropFilter), assets/ with 14 PNG images.

Request: Review the repo for performance problems but don't change anything yet.
