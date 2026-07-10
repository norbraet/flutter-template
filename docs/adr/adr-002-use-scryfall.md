# ADR-002: Use Scryfall as the canonical card database

## Status

Accepted

## Context

Deck building, card lookup, printings, images, legalities, faces, and rulings require structured Magic card data. Rebuilding and maintaining this catalog is outside the product's purpose. Live games must remain usable offline and must not depend on external request latency.

## Decision

Use Scryfall as the canonical upstream card-data provider behind an internal `CardCatalog` interface. Normalize and cache needed records locally. Store OrbiGathering-owned decks, printing choices, games, and settings in the application database. Use direct API access initially and retain a path to Scryfall bulk data or a backend index.

## Consequences

- The app benefits from stable Oracle/printing identifiers, structured card fields, rulings, images, and bulk exports.
- Scryfall terms, attribution, image requirements, headers, and rate limits must be reviewed and respected.
- Search needs throttling, cancellation, batching, error handling, and caching.
- Upstream availability cannot block gameplay; selected-deck data is cached for offline access.
- The adapter and normalized model add implementation work but isolate schema and policy changes and permit future alternative sources.

