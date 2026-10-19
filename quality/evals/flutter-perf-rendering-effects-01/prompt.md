---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
// lib/shared/glass_card.dart
BackdropFilter(filter: ImageFilter.blur(sigmaX: 12, sigmaY: 12), child: card)
// lib/features/dashboard/fade_in.dart
Opacity(opacity: _anim.value, child: child) // inside an AnimatedBuilder
// lib/features/transactions/disabled_row.dart
Opacity(opacity: 0.4, child: row)
```

Request: Remove all BackdropFilter and Opacity widgets to be safe.
