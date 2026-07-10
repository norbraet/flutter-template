# Gameplay model

## Scope

OrbiGathering models companion-managed state, not the complete battlefield and not every Magic rule. It tracks the information needed around physical cards and asks players to report only facts the app cannot infer.

## Turn structure

The engine represents all twelve steps internally:

1. Untap
2. Upkeep
3. Draw
4. First main phase
5. Beginning of combat
6. Declare attackers
7. Declare blockers
8. Combat damage
9. End of combat
10. Second main phase
11. End step
12. Cleanup

The UI may expose these individually in guided mode or group them into beginning, first main, combat, second main, and ending in compact mode. Players may switch modes during a game.

The tracker owns the active player, turn and round numbers, phase and step, elapsed turn time, and total duration. Controls include next/previous phase, end turn, pause/resume, change active player, and undo.

Ending a turn advances the active player, increments turn or round, clears temporary resources, expires appropriately marked effects, resets once-per-turn markers such as “land played,” surfaces end-step reminders, and prepares beginning-of-turn reminders. The [architecture](architecture.md) describes how these consequences become domain events.

## Guided and compact modes

Guided mode shows individual steps with short, curated explanations:

```text
Upkeep

Resolve abilities that trigger at the beginning of your upkeep.
```

Compact mode shows the current major phase and favors rapid advancement. Detailed phase tracking can be disabled entirely. A small offline glossary covers flying, vigilance, lifelink, trample, deathtouch, first strike, double strike, reach, haste, hexproof, ward, menace, flash, defender, summoning sickness, priority, the stack, permanents, tokens, and counters.

The glossary is curated rather than generated during play. App explanations are clearly distinguished from official Oracle text and published rulings.

## Formats and setup

Rules vary, so starting life, players, teams, counters, and phase detail come from configurable presets rather than deck size or hard-coded assumptions. Initial presets are:

- 60-card one-on-one;
- Commander, commonly multiplayer with 40 starting life;
- Two-Headed Giant, with teams and a shared starting life commonly set to 30;
- Custom casual games and house rules.

Setup chooses player count and names, preset or custom rules, starting life, turn order, a selected deck for each locally managed player, phase-detail preference, timer, and visible counters. The app must not infer a format from deck size; several formats use 60 cards and legality rules vary.

## Life and players

Life supports tap increment/decrement, hold or swipe for larger changes, direct numeric entry, configurable step size, animation, haptics, multiple players, rotated layouts, accidental-large-change protection, history, and undo.

Each update is explicit:

```text
Player A lost 3 life
Player B gained 2 life
Player A’s life was set to 10
```

The active player is visually prominent. Player panels expose only the counters and state relevant to the chosen format and current context.

## Counters and resources

The counter system is generic. Built-ins may include poison, energy, experience, commander tax, commander damage by opposing commander, initiative, monarch, day/night, and custom counters. Presets decide defaults.

A custom counter has a name, icon, value, minimum, increment, optional owner, optional color, and optional description. Some concepts such as monarch, initiative, and day/night are shared or exclusive state but can use the same extensible presentation and event mechanisms.

## Reminders

Reminders are a major value beyond a life counter. A reminder includes title, optional description, owner, trigger, expiration and repetition behavior, related card, creation time, and completion state.

Initial triggers include:

- beginning of the owner's next turn;
- next upkeep or draw step;
- beginning or end of combat;
- next end step or end of the current turn;
- after a selected number of turns;
- manual, without automatic trigger.

Examples:

```text
Next upkeep: lose 1 life.
End of turn: remove the temporary +2/+2 effect.
Next combat: this creature must attack if able.
After two turns: remove the time counter.
```

At the appropriate transition, the app surfaces the reminder; it does not automatically resolve a physical action. From a selected deck card, a player can choose a relevant ability or enter text, select a trigger, and save. Version 1 does not translate arbitrary Oracle text into executable rules.

## Active effects

An active effect has a name, optional source card, affected player or free-form object label, start turn, duration, expiration point, notes, status, and optional numeric modifier. Supported duration patterns include until end of turn, until the owner's next turn or upkeep, rest of game, while a source remains in play, and manual removal.

Useful physical labels avoid a full battlefield model:

```text
Left Angel token
Giada
Opponent’s largest creature
All Angels
```

Deterministic expirations happen at transitions. “While source remains in play” and other unknowable conditions require confirmation.

## Decks and in-game card access

Players can create, name, format, duplicate, archive, import, and export decks; add/remove cards and quantities; separate main deck and sideboard; choose a printing; and store notes. The lightweight builder provides search, mana curve, color and type distributions, land count, basic validation, and deck-specific reminder templates.

Warnings are guidance, never an absolute guarantee: too few cards, too many non-basic copies, possible format illegality, oversized sideboard, or commander color-identity conflict. Legality and special rules change.

During a game, selected-deck access prioritizes recent cards, deck-only search, creatures, lands, instants, sorceries, and permanents. This avoids searching the entire database for every interaction. Card detail shows image, mana cost, type, Oracle text, power/toughness or loyalty, keywords, set and collector number, legalities, related faces, rulings, and attribution.

## Timers

The optional timer tracks total game duration, current-turn duration, configurable warnings, pause/resume, and accumulated player turn time. It is not a competitive chess clock by default and can be disabled when distracting.

## History, recovery, and game flow

Every meaningful action enters history for undo, redo, recovery, summaries, debugging, and future synchronization. Version 1 need not expose full replay UI, but the model supports it.

A complete baseline game flow is:

1. Choose preset, players, turn order, optional decks, counters, phase detail, and timer.
2. Start the game and persist its initial event and snapshot.
3. Adjust life/counters, advance phases, and create reminders/effects as needed.
4. Surface reminders and expire deterministic effects on state transitions.
5. End, pause, or leave the game; persist every meaningful action.
6. Resume the exact state after app or device restart.
7. Undo any important action, including end turn.

