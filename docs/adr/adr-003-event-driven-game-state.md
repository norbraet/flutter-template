# ADR-003: Use event-driven local-first game state

## Status

Accepted

## Context

Players enter changes quickly, advance phases accidentally, and revise actions after rules discussions. A live game must survive app termination and device restarts. Future multiplayer needs ordered, idempotent changes and reconnect support. Direct destructive mutation makes undo, history, replay, diagnosis, and synchronization difficult.

Full event sourcing is unnecessary for decks and settings, but live-game state benefits directly from an event history.

## Decision

Represent every meaningful live-game action as a versioned domain event. Derive current state from a compatible snapshot plus later events, persisting every meaningful action locally in SQLite/Drift. Commands go through a domain-specific `GameEngine`, which validates transitions and emits deterministic consequences. Use ordinary CRUD for domains that do not benefit from event history.

## Consequences

- Undo, redo, history, crash recovery, development replay, and future synchronization share one foundation.
- Ending a turn can atomically emit all inferred cleanup, expiry, reminder, and active-player changes.
- Event payloads, reducers, ordering, transactions, snapshot policy, migrations, and upcasting require disciplined tests.
- Derived state may be materialized for efficient queries but must remain consistent with events.
- Multiplayer can add device actors, authority, server sequences, idempotency, and missing-event recovery without replacing the Version 1 model.

