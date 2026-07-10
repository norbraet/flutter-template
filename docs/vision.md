# Product vision

## Executive summary

OrbiGathering is a smartphone companion for physical games of Magic: The Gathering, initially targeting iOS and Android. It manages information that is easy to forget, hard to represent clearly on the table, or inconvenient to track with dice, paper, and unrelated applications.

> The physical table represents the cards. The companion represents the invisible game state.

The app tracks life, turn order, phases and steps, counters and resources, temporary effects, reminders, delayed triggers, timers, selected decks, and frequently relevant card information. It does not require players to reproduce physical actions digitally: tapping a land on the table must not require tapping the same land in the app.

Version 1 is an offline-first, account-free, single-device application. Players can configure decks, start and recover games, manage player information, inspect cards, and use a contextual game dashboard. The longer-term direction is a nearby shared session in which participants manage their own information while public state is synchronized.

## Product vision

The long-term vision is a heads-up display for tabletop Magic. Like a video-game HUD, it does not duplicate the world; it surfaces current health, resources, effects, objectives, timers, warnings, and contextual actions.

OrbiGathering should quickly answer:

- Whose turn is it, and which phase or step is active?
- Has a land already been played this turn?
- Which temporary effects are active, and when do they expire?
- Is there a reminder for the next upkeep, combat, or end step?
- How and when did a life total change?
- What does a keyword mean, or what ruling applies to a card?
- Which printing of a card is in a selected deck?
- What happened during the previous turn?

It should not attempt to decide every legal action or resolve arbitrary card interactions. A complete Magic rules engine is a different product.

## Product positioning

OrbiGathering is:

> A smart game-state companion for tabletop Magic.

Its distinction from a conventional life counter is contextual assistance. A normal counter shows a number and generic controls. OrbiGathering knows that combat is active, a reminder is pending, and a particular deck is selected, so it can prioritize relevant information without modeling the battlefield.

It is not initially:

- a replacement for Magic Arena or a digital battlefield;
- an authoritative rules judge or strategic adviser;
- a camera scanner or automatic battlefield recognizer;
- a complete collection, pricing, trading, or deck-market platform;
- a tournament administration system, matchmaking service, or social network.

## Target users

### Primary: beginners

New tabletop players are still learning turn structure, keywords, common resources, trigger timing, temporary versus permanent effects, and what must be tracked. They benefit from explanations, reminders, safe defaults, and forgiving undo.

### Secondary: casual regular players

Experienced casual players need fast life totals, multiplayer layouts, counters, commander damage, poison, turn tracking, history, deck access, and card rulings without beginner guidance slowing play.

### Not initially optimized for

Version 1 is not primarily for tournament judges, professional players, large event organizers, online-only players, market analysts, or users expecting automatic battlefield recognition.

## Product philosophy

The app should reduce bookkeeping, not make users maintain the same game twice. It records invisible state and accepts only information that cannot be inferred from physical play. Contextual quick actions should keep ordinary turns low-touch and glanceable.

The app works without an account: install, create or import a deck, start a game, and use the complete core experience offline. Accounts, remote storage, and synchronization remain optional future capabilities.

Version 1 minimizes privacy risk. Player nicknames, decks, histories, and preferences remain local by default. It should not request contacts, precise location, camera, or Bluetooth access. Analytics should be opt-in or privacy-preserving; users need a “delete all local data” action. Crash reports must not automatically include decks or game state, and every third-party service must be documented before release.

## Design principles

### Complement the table

Do not reproduce information already obvious from physical cards. A detailed tracking mode may be optional, but a complete digital copy of lands, creatures, and hands is not a baseline requirement.

### One action updates all deterministic consequences

Ending a turn can advance the active player and turn count, clear temporary mana, expire “until end of turn” effects, reset “land played,” surface end-step reminders, and prepare the next player's beginning-of-turn reminders.

### Ask only when information cannot be inferred

The app can expire an effect the user marked “until end of turn” and reset once-per-turn markers. It cannot know that a creature died or which spell was cast unless told.

### Make undo permanent and obvious

Every game-state action must be reversible. Life changes are fast, phases are easy to advance accidentally, and players frequently correct actions after rules discussions. The event history is therefore a core product capability, not an implementation detail.

### Prioritize glanceability

Use large values, strong hierarchy, high contrast, generous touch targets, minimal in-game text, one-handed controls, and optional haptic confirmation. Players should look primarily at the table.

### Serve both learning and speed

Guided and compact modes, optional phase tracking, optional reminders, and saved preferences prevent beginner assistance from burdening experienced users.

## Product risks and mitigations

- **Excessive manual input:** track only invisible state, measure taps per turn, and remove features that cost more bookkeeping than they save.
- **Becoming a rules engine:** present Oracle text and rulings as reference, surface reminders rather than resolving arbitrary effects, and never make Version 1 an authority on legality.
- **Scryfall dependency:** cache deck cards and viewed rulings, persist normalized records, support offline play, and isolate the API behind an adapter.
- **Scope expansion:** keep written non-goals and optimize deck building for game preparation rather than prices, collections, or social sharing.
- **Beginner UI frustrating regulars:** make guidance, detailed phase tracking, and reminders optional while keeping the life counter fast.

## Success metrics

Success is measured by whether the app reduces friction, not by feature count:

- time required to start a game;
- average interactions per player turn;
- successful game-recovery rate;
- undo rate and whether mistakes are easy to correct;
- reminders created per game;
- users who create or import a deck;
- card-search response time;
- crash-free game sessions;
- users who complete a second game;
- qualitative answers to “Did the app reduce or add bookkeeping?”

The central measure is:

> During an ordinary turn, how often did the player need to touch the application?

The ideal is not zero, but it should remain low.

