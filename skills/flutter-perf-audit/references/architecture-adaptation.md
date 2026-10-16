# Architecture Adaptation

Contents: Principle · Detect · Map techniques · Output

## Principle

Recommendations must fit the project in front of you. Detect its architecture first, then express every fix with that project's own mechanisms. Never recommend a different state-management package or layer structure to apply a technique.

## Detect

Check `pubspec.yaml`, `lib/` layout and a few representative screens for:

| Signal | Conclusion |
|---|---|
| `flutter_riverpod` / `riverpod` | Riverpod |
| `flutter_bloc` / `bloc` | Bloc/Cubit |
| `provider` | Provider |
| `get` | GetX |
| `mobx`, `signals`, `redux` | note it; use that library's own fine-grained observation |
| none of the above | `setState`, `ValueNotifier`, `InheritedWidget` |
| `go_router`, `auto_route`, `Navigator` | navigation approach (affects lifecycle checks) |
| `dio`, `http`, `retrofit`, `graphql` | networking layer (where pagination and caching can live) |
| `drift`, `sqflite`, `isar`, `hive`, `objectbox` | local store (can aggregate or paginate locally) |

Also record folder style (feature-first vs layer-first), existing performance utilities, and lint config. Only state what you observed.

## Map techniques

| Goal | Riverpod | Bloc/Cubit | Provider | Plain Flutter |
|---|---|---|---|---|
| Rebuild only on one field | `ref.watch(p.select(...))` | `BlocSelector` or `buildWhen` | `Selector` or `context.select` | split into smaller widgets; `ValueNotifier` per field |
| Localise a rebuild | `Consumer` around the dependent part | `BlocBuilder` around the dependent part | `Consumer` around the dependent part | `ValueListenableBuilder` / small `StatefulWidget` |
| Cache a derived value | derived `Provider` keyed on inputs | compute in the Cubit/Bloc state or a repository | derived value in the model/`ProxyProvider` | compute in the controller when inputs change |
| Per-item state | `family` providers | per-item Cubit or item widget state | per-item `ChangeNotifier` or widget state | item widget state |

Check the installed package version's docs before using an API you are unsure of.

## Output

In the audit's plan, state the detected architecture once, then phrase each `<change>` in those terms (e.g. "add `buildWhen` to the `BlocBuilder` in `X`" rather than a generic "narrow the subscription"). If the architecture is unclear or mixed, say so and ask rather than guess.
