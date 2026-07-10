# ADR-001: Use Flutter as the application framework

## Status

Accepted

## Context

OrbiGathering targets iOS and Android and needs custom, animated, highly glanceable game controls, strong offline behavior, and a consistent fantasy-inspired visual system. It is more visually specialized than a typical CRUD app but does not require a full game engine.

The original briefing explored both Flutter and React Native/Expo. Maintaining two competing recommendations would prevent a coherent architecture and delivery plan.

## Decision

Build OrbiGathering with Flutter and Dart. Use Flutter's rendering and animation systems, `CustomPainter` for signature visuals, and selected Rive assets where they add material value. Use a domain-specific Dart game engine rather than XState.

## Consequences

- Android and iOS share one codebase and a consistent rendering model.
- The team gains precise control over custom visuals, gestures, and animation.
- Domain and persistence code are implemented in Dart, and the React Native/Expo/TypeScript proposal is superseded.
- Contributors need Flutter and Dart expertise, and platform-specific integration still requires native testing.
- The UI must retain mobile accessibility and conventions despite its custom styling.

