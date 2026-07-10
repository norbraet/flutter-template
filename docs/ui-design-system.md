# UI design system

## Experience goals

OrbiGathering should feel like a premium fantasy-game HUD while remaining fast, familiar, and accessible as a mobile application. The target balance is approximately 80% clean modern interface and 20% fantasy atmosphere.

Visual inspiration includes Magic Arena, Hearthstone, and Gwent, but the product must not imitate their interfaces. Use dark stone-like surfaces, restrained gold accents, subtle magical glow, layered panels, elegant typography, and limited ornamentation. Avoid heavy parchment textures, decorative body fonts, and effects that compete with game state.

## Interaction principles

- Keep attention on the physical table through large values, few controls, and short labels.
- Put one contextual primary action on the game dashboard.
- Make undo visible after every meaningful action.
- Prefer deterministic automation over repeated bookkeeping.
- Design for one hand and for players seated on opposite sides of a table.
- Provide immediate visual and optional haptic feedback.
- Prevent or make it easy to undo accidental large life changes and destructive actions.

## Foundations

### Color

Define semantic tokens rather than hard-coded widget colors. The base theme uses near-black stone backgrounds, elevated charcoal panels, warm gold accents, and readable neutral text. Add tokens for positive changes, damage or danger, warnings, focus, disabled state, and magical emphasis.

Color must never be the only carrier of meaning. Every state also needs text, iconography, shape, or position. A high-contrast theme is required.

### Typography

Use an elegant display face sparingly for identity and major headings, and a highly legible scalable face for values, labels, explanations, and card text. Life totals need tabular or visually stable numerals. Decorative type must never reduce scan speed.

### Spacing and layout

Use a documented spacing scale and consistent panel insets. Generous touch separation matters during fast tabletop interactions. Layouts should adapt to phone sizes, landscape orientation where useful, system text scaling, and rotated player panels.

### Icons and imagery

Icons must have screen-reader labels and recognizable silhouettes. Magic-inspired decoration should remain original and restrained. Card artwork keeps its original proportions and attribution; image-specific requirements are documented in [Scryfall integration](api.md).

## Component philosophy

Build the UI from internal primitives rather than directly composing inconsistent Material widgets:

- `OrbiButton` — primary, secondary, quiet, and destructive actions with large targets.
- `OrbiPanel` — layered surface with consistent border, elevation, padding, and states.
- `OrbiDialog` — accessible confirmations, numeric entry, and focused creation flows.
- `PlayerPanel` — name, life, relevant counters, active-player state, and optional rotation.
- `LifeOrb` — signature large-value control with tap, hold/swipe, direct entry, animation, and haptics.
- `PhaseTracker` — guided step or compact phase, current context, previous/next, and end-turn affordances.
- `CounterChip` — labeled icon and value with quick adjustment.
- `ReminderCard` — trigger, owner, source, description, and completion action.
- `CardPreview` — correctly proportioned thumbnail with structured accessible text.

Components expose semantic states and accessibility behavior; styling is driven by design tokens.

## Motion

Animation communicates cause, state change, and hierarchy; it must never delay gameplay.

| Motion | Target duration |
| --- | ---: |
| Immediate feedback | 120 ms |
| Standard transition | 250 ms |
| Page transition | 350 ms |
| Life change | 250–300 ms |
| Victory moment | 800 ms |

Life changes should make direction and amount apparent without obscuring the new total. Phase changes should preserve orientation. Premium Rive animation is selective; routine motion uses Flutter transitions. Reduced-motion mode replaces nonessential movement with fades or immediate state changes.

## Key layouts

### Home

The primary action is resuming or starting a game:

```text
orbi-gathering

[ Resume current game ]
[ Start game ]

My Decks
- Angels
- Starter Deck

Recent Games
- Angels vs. Alex
```

### Game dashboard

```text
TURN 5 • YOUR TURN
FIRST MAIN PHASE                  12:43

                 24
              YOUR LIFE

Opponent                         16

Land played                      No
Active effects                    2
Upcoming reminders                1

[ Add ]       [ Next phase ]

Undo: You gained 3 life
```

The Add sheet contains change life, add counter, add effect, add reminder, look up card, and add note. During upkeep or combat, the dashboard replaces generic content with immediate reminders and actions:

```text
UPKEEP

2 reminders
• Gain 1 life
• Remove a counter from Angelic Accord

[ Complete all ]       [ Next ]
```

```text
DECLARE ATTACKERS

No mandatory reminders.

[ Skip combat ]        [ Next ]
```

The app does not ask users to identify every attacker.

### Deck and card detail

The deck overview prioritizes card count, type distribution, mana curve, and start/edit actions. Card detail prioritizes the unaltered physical image and official rules text, followed by add to deck, create reminder, rulings, printing selection, and personal notes.

## Accessibility

Accessibility is a Version 1 requirement:

- screen-reader names, roles, values, and hints for every control;
- life totals announced with player names;
- structured card name and Oracle text separate from inaccessible image text;
- no color-only information;
- scalable text and reflow at supported system sizes;
- generous touch targets and spacing;
- reduced motion and high contrast;
- optional haptics;
- landscape support where useful;
- consideration for left- and right-handed use;
- confirmation or prominent undo for destructive actions.

