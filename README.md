# flutter-template

A Flutter project template with a pre-wired toolchain and GitHub project-management baseline. It provides the scaffolding — task runner, git hooks, commit/branch conventions, CI, issue forms, labels, milestones, a GitHub Project, a default-branch ruleset, and automated releases — so a new Flutter project can start from a working setup instead of an empty repository.

Use this repository as a starting point: clone or use it as a GitHub template, then rename the app (see below), fill in `docs/` with your product's actual vision and architecture, and adjust `.github/` labels, milestones, and issue-form copy to your project.

See the [project documentation](docs/README.md) for the documentation structure this template expects you to fill in.

## Development

The project uses [mise](https://mise.jdx.dev/) for Flutter and Lefthook versions as well as common development tasks.

After cloning and generating the Flutter application:

```sh
mise install
mise run setup
```

The GitHub bootstrap task also requires the [GitHub CLI (`gh`)](https://cli.github.com/). Authenticate it before synchronizing labels, milestones, or project settings:

```sh
gh auth login -h github.com
gh auth refresh -s project
mise run github:bootstrap -- --dry-run
```

Remove `--dry-run` when you are ready to apply the changes.

Common commands:

```sh
mise run format       # Apply Dart formatting
mise run analyze      # Run static analysis
mise run test         # Run unit and widget tests
mise run check        # Verify formatting, analysis, and tests
mise run doctor       # Inspect the local Flutter toolchain
mise run pr:create    # Create a PR using the latest Conventional Commit message
```

Commits follow the Conventional Commits format, for example
`feat(auth): add sign-in screen`. The commit hook validates this format, and
`mise run pr:create` uses the latest commit message to prefill the pull request
title while retaining the repository pull request template as the body. For a
branch such as `feat/123-sign-in-screen`, it also adds `Closes #123` to link and
close the issue when the pull request merges.

Issue forms use matching title prefixes: `feat:`, `fix:`, and `chore:`. GitHub
uses those titles when suggesting a branch from an issue. When creating a
branch manually, use the project convention `<type>/<issue>-<description>`,
such as `feat/123-sign-in-screen` or `fix/124-null-crash`.

## Branching model

`develop` is the default branch and the target for ordinary feature/fix pull requests; both `main` and `develop` are protected (no direct pushes, no force-push, no deletion) by the ruleset `mise run github:bootstrap` synchronizes. `main` only moves when you deliberately promote `develop` into it:

```sh
mise run release:promote
```

That opens a `develop → main` pull request. Merging it is what triggers [release-please](.github/workflows/release-please.yml), which maintains a "Release PR" bumping `pubspec.yaml`'s version and `CHANGELOG.md` from `feat`/`fix`/breaking-change commits since the last release; merging *that* PR cuts a tagged GitHub Release. GitHub cannot technically restrict which branch a pull request comes from, so keeping `main` release-only is a convention this template documents, not a hard gate — see `.github/PROJECT_MANAGEMENT.md`.

## Renaming the app

This template ships with a placeholder identity: pubspec package `flutter_template`, Android namespace/applicationId `com.example.flutter_template`, and iOS bundle identifier `com.example.flutter_template`. Before shipping, replace these with your own values in `pubspec.yaml`, `android/app/build.gradle.kts`, `android/app/src/main/AndroidManifest.xml`, `android/app/src/main/kotlin/com/example/flutter_template/MainActivity.kt` (move the file to match your new package), `ios/Runner/Info.plist`, and `ios/Runner.xcodeproj/project.pbxproj`.

## Running the application

This template currently targets Android and iOS only — that was a deliberate scope choice for the original project it was extracted from, not a technical limit. To add desktop or web support, run the following from the repository root and commit the generated platform folders:

```sh
flutter create --platforms=web,macos,linux,windows .
```

Only add the platforms you actually intend to support and test; each one adds native project files you're responsible for keeping current.

### Windows and Android

Windows development requires either an Android emulator or a physical Android phone. Android Studio and its Android SDK are installed separately on each development machine.

To use an emulator, create and start an Android Virtual Device in Android Studio's **Device Manager**, then verify that Flutter can see it:

```powershell
mise run emulators
mise run devices
mise dev
```

You can launch an emulator from PowerShell when you know its ID:

```powershell
mise run emulators -- --launch <emulator-id>
mise dev
```

To use a physical Android phone, enable **Developer options** and **USB debugging**, connect the phone over USB, accept the debugging authorization prompt, and run:

```powershell
mise run devices
mise dev
```

If more than one device is available, target one explicitly:

```powershell
mise dev -- -d <device-id>
```

If Flutter reports that only Windows or web devices are available, start an Android emulator or connect an authorized Android phone. Do not run `flutter create .` solely to make those unsupported devices appear; that would expand the platform scope.

### iOS

iOS development requires macOS and Xcode. Use either an iOS Simulator started from Xcode or an authorized physical iPhone connected to the Mac. Then run:

```sh
mise run devices
mise dev
```

The iOS simulator and iOS build tools are not available from Windows 11.
