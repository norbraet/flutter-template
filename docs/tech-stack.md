# Technology stack

This document reflects what is actually in `pubspec.yaml` today. As the project adds real dependencies, add a section here for each one following the same pattern: what it's for, and why it was chosen over the alternatives.

## Framework: Flutter

One Android/iOS codebase from a single Dart source tree, with a mature widget/rendering system and animation support. See [ADR-001](adr/adr-001-use-flutter.md).

## Language: Dart

Dart is Flutter's first-class language: sound null safety, static analysis, and code generation support. Strict analysis is enabled via `flutter_lints` and `analysis_options.yaml` from the start.

## Linting: flutter_lints

The only non-SDK dependency in this template. Provides the recommended lint set enforced by `mise run analyze` and `mise run check`. Extend `analysis_options.yaml` deliberately rather than suppressing lints broadly — see `AGENTS.md`.

## Placeholder: state management and dependency injection

Not yet chosen. When you pick one (e.g. Riverpod, Bloc, Provider, plain `InheritedWidget`/ChangeNotifier), document it here: what it's responsible for (application/UI state), and what it explicitly does not own (domain rules — see [architecture](architecture.md)).

## Placeholder: persistence

Not yet chosen. When you pick a local persistence solution (e.g. Drift/SQLite, Isar, Hive, shared_preferences for simple settings), document what it stores, how migrations are handled, and how durability is verified — see [data model](data-model.md).

## Placeholder: networking

Not yet chosen, and only needed if the project talks to an external service. When you pick an HTTP client (e.g. Dio, http), document how requests are debounced/cancelled/retried and where that logic is centralized — see [external integration](api.md).

## Placeholder: navigation

Not yet chosen. When you pick a routing solution (e.g. GoRouter, Navigator 2.0 directly), document how it represents the app's top-level flows without leaking navigation concerns into domain code.

## Testing strategy

- Unit tests for domain logic, policies, and validation, without Flutter bindings.
- Widget tests for interaction and semantics; golden tests only for stable, visually important states.
- Integration tests for persistence, migrations, and (once infrastructure exists) end-to-end flows.
- Fixtures and fakes for any external-service adapter; normal CI must not depend on live third-party access.

## Stack summary

```text
Framework:        Flutter
Language:         Dart
Linting:          flutter_lints
State/DI:         (not yet chosen)
Persistence:      (not yet chosen)
Networking:       (not yet chosen)
Navigation:       (not yet chosen)
Backend/auth:     (not yet chosen)
```
