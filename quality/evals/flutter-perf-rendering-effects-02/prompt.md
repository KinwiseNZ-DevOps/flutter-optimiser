---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

Frame timeline shows slow raster frames, not slow UI frames, on the dashboard.
```dart
ListView(children: [
  for (final card in cards) ClipRRect(borderRadius: BorderRadius.circular(16),
    child: BackdropFilter(filter: ImageFilter.blur(sigmaX: 14, sigmaY: 14), child: GlassCard(card))),
])
```

Request: Raster frames are slow on the dashboard with a blurred glass card in a scrolling list.
