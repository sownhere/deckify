# Deckify

Deckify turns your iPhone/iPad into a wireless trackpad and macro deck for your Mac with ultra-low latency — no complex Wi-Fi router setup, no cloud dependency, no dedicated hardware like Magic Trackpad or an Elgato Stream Deck.

## Vision

Make the most of the mobile devices you already own to build a complete wireless computer-control ecosystem. Deckify aims to make the workspace minimal and flexible while still matching the performance and responsiveness of premium physical peripherals.

## Problem

- **Cost and desk space**: professionals pay a lot of money and sacrifice desk space for single-purpose hardware (Magic Trackpad, Elgato Stream Deck).
- **Limitations of existing solutions**: apps that turn a phone into a mouse/keyboard usually rely on web protocols or heavy network stacks, resulting in high latency, choppy input, and frequent disconnects.
- **Wasted potential**: the processing power and high refresh rate of iPhone, and the resolution and Apple Pencil support of iPad, are not being leveraged for direct computer interaction.

## Solution

An app that runs entirely over the local network with a low-level network architecture:

- **UDP** for the trackpad data stream, aiming for near-zero latency by bypassing the Internet/cloud entirely.
- **Clean Architecture** so the app can easily grow from a simple trackpad into a full multi-function macro pad system.

## Target audience

- **Early stage**: developers and engineers who need quick input or want to control a second machine (e.g. a Mac mini) without plugging in extra keyboard/mouse.
- **Later stage**: designers, video editors, and streamers who want a customizable shortcut control panel (macro pad) right on their iPad/iPhone.

## Roadmap

### Phase 1 — The Core Trackpad

Focus entirely on smoothness. Build a stable UDP bridge between the iOS client and the macOS server.

- Move the cursor via `DragGesture` with an acceleration interpolation algorithm.
- Ultra-responsive click actions (left/right/double).
- Absolute positioning mode, optimized for iPad + Apple Pencil users, turning the screen into a 1:1 mapped drawing tablet for the Mac's display.

### Phase 2 — The Macro Deck

Add TCP to guarantee 100% accurate command delivery.

- Grid-based UI for buttons (Stream Deck style).
- Sync a config file (JSON) between Mac and iOS to customize each button's color, icon, and command.
- Support triggering system shortcuts or shell scripts on macOS.

### Phase 3 — Automation & Discovery

A "zero-config" experience for end users.

- Integrate Bonjour (service discovery) so client and server automatically find each other, no manual IP entry required.

## Tech stack

| | |
|---|---|
| **iOS Client** | Swift, SwiftUI, `Network.framework` (`NWConnection`) |
| **macOS Server** | Swift, AppKit/SwiftUI (runs as a menu bar background app), `Network.framework` (`NWListener`), CoreGraphics (`CGEvent`) |

This repo (`deckify`) contains the iOS client.

### Dependencies

- [swift-dependencies](https://github.com/pointfreeco/swift-dependencies) — dependency injection.
- [swift-navigation](https://github.com/pointfreeco/swift-navigation) — SwiftUI navigation.
- [swift-clocks](https://github.com/pointfreeco/swift-clocks) — controllable clocks for testing (used in `DeckifyTests`).

## Requirements

- Xcode 26.0+
- iOS 26.0+ (deployment target of the `Deckify` target)
- [SwiftLint](https://github.com/realm/SwiftLint) and [SwiftFormat](https://github.com/nicklockwood/SwiftFormat) — run automatically in a pre-build script; not required but recommended to avoid build warnings

```bash
brew install swiftlint swiftformat
```

## Setup & running the project

```bash
open deckify.xcodeproj
```

Pick the scheme matching the environment you want to run (see Environments & Schemes), then Run (`Cmd+R`).

## Environments & Schemes

Deckify has 3 environments, each with its own config file under [envs/](envs):

| Environment | Bundle ID | Config file |
|---|---|---|
| Dev | `com.sownhere.deckify.dev` | [envs/dev.xcconfig](envs/dev.xcconfig) |
| Staging | `com.sownhere.deckify.staging` | [envs/staging.xcconfig](envs/staging.xcconfig) |
| Production | `com.sownhere.deckify` | [envs/production.xcconfig](envs/production.xcconfig) |

Each environment has its own Debug and Release scheme (e.g. `Deckify Dev Debug`, `Deckify Staging Release`, ...). `*Debug` schemes build the test target as well; `*Release` schemes only build the app, used for archiving/profiling.

## Project structure

``` struct
deckify/
├── project.yml              # XcodeGen config used to scaffold the project once (no longer regenerated; `deckify.xcodeproj` is now the source of truth)
├── envs/                     # xcconfig per environment
├── Deckify/
│   ├── Sources/              # App's SwiftUI code (entry point: DeckifyApp.swift)
│   └── Resources/            # Info.plist, entitlements, asset catalog
├── DeckifyTests/             # Unit test target
└── Shared/                   # Shared code (currently empty)
```

## Build, test & lint

```bash
# Build (pick the appropriate scheme)
xcodebuild -project deckify.xcodeproj -scheme "Deckify Dev Debug" build

# Run unit tests
xcodebuild -project deckify.xcodeproj -scheme "Deckify Dev Debug" test

# Lint / format manually (already runs automatically in the pre-build script when building via Xcode)
swiftformat .
swiftlint
```

## Architecture

The project follows Clean Architecture to separate the network layer (UDP/TCP), the use-case layer (gesture and macro handling), and the UI layer (SwiftUI). Detailed layering will be fleshed out as the network/domain modules are built in Phase 1.

## Contributing

This is an open-source project — contributions are welcome.

- Branch off `main`, naming branches `feature/<short-description>` or `fix/<short-description>`.
- Make sure `swiftlint` is clean and `xcodebuild test` passes before opening a PR.
- Project configuration changes (target, scheme, dependency) are made directly in Xcode / `deckify.xcodeproj`.

## License

[MIT](LICENSE)
