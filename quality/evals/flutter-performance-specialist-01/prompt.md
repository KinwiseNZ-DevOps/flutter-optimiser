---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

Users report dropped frames on mid-range phones. The dashboard screen shows a blurred glass card above a scrolling list of about 500 transactions, and the transactions screen has a search box above a list.

Request: Our Flutter app feels janky. Make it faster.
