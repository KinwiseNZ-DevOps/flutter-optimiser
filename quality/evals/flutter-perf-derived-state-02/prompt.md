---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

Existing code in lib/features/budgets/budget_math.dart: a few dozen lines that add up planned and actual amounts for about 12 budget lines and format percentages for display. The Django backend already returns the transaction list; it has no totals endpoint.

Request: Move all of these calculations to the backend.
