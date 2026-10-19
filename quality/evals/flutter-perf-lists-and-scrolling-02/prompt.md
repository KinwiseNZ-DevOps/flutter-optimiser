---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): a Flutter personal-finance app called ledger-app, state in Riverpod, screens under lib/features/transactions, lib/features/dashboard and lib/features/settings, shared widgets under lib/shared. Answer from this description and the code below.

```dart
ListView(children: const [
  SettingsRow('Profile'), SettingsRow('Currency'), SettingsRow('Notifications'),
  SettingsRow('Security'), SettingsRow('Export'), SettingsRow('About'),
])
```

Request: Convert the settings screen's 6 rows to ListView.builder.
