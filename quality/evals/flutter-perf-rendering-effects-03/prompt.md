---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

The app has 9 screens, most of them static forms or lists with no animation. Two screens have a spinner animation and one has a chart that redraws every second.

Request: Wrap every screen in RepaintBoundary.
