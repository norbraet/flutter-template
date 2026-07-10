# orbi-gathering

A modern companion app for physical Magic: The Gathering, focused on game-state management, deck organization, and an intuitive tabletop experience.

See the [project documentation](docs/README.md) for the product vision, gameplay model, architecture, technology decisions, and roadmap.

## Development

The project uses [mise](https://mise.jdx.dev/) for Flutter and Lefthook versions as well as common development tasks.

After cloning and generating the Flutter application:

```sh
mise install
mise run setup
```

Common commands:

```sh
mise run format       # Apply Dart formatting
mise run analyze      # Run static analysis
mise run test         # Run unit and widget tests
mise run check        # Verify formatting, analysis, and tests
mise run doctor       # Inspect the local Flutter toolchain
```

Android Studio and its Android SDK are installed separately on each development machine. Building and testing the iOS application additionally requires macOS and Xcode.
