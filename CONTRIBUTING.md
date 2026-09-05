# Contributing to Deckify

Thanks for your interest in contributing! This is an early-stage open-source project — expect the architecture to still be evolving.

## Getting started

1. Fork & clone the repo.
2. Open `deckify.xcodeproj` in Xcode 26+.
3. Install SwiftLint & SwiftFormat (optional but recommended): `brew install swiftlint swiftformat`.
4. Pick the scheme matching the environment you want to run — see [Environments & Schemes](README.md#environments--schemes).

## Branching & commits

- Branch off `main`: `feature/<short-description>` or `fix/<short-description>`.
- Write commit messages in the imperative mood (e.g. "Add UDP cursor stream", not "Added" or "Adding").
- Keep commits focused — one logical change per commit.

## Before opening a PR

- `swiftlint` runs clean (no errors).
- `xcodebuild -project deckify.xcodeproj -scheme "Deckify Dev Debug" test` passes.
- New business logic (network, use-case layers) has unit test coverage in `DeckifyTests`.
- Project configuration changes (target, scheme, dependency) are made directly in Xcode / `deckify.xcodeproj` — `project.yml` was only used to scaffold the project once and is no longer regenerated.

## Pull requests

- Fill out the PR template — what changed, why, and how you tested it.
- Keep PRs scoped to one change; avoid bundling unrelated refactors.
- CI (lint + build + test) must pass before merge.

## Architecture

Deckify follows Clean Architecture: network layer (UDP/TCP) → use-case layer (gesture/macro handling) → UI layer (SwiftUI). Keep new code within these boundaries — see [Architecture](README.md#architecture) for details as they're fleshed out.

## Code style

- SwiftFormat/SwiftLint run automatically as an Xcode pre-build script — fix warnings before committing.
- Prefer `struct`s and value types where possible; keep SwiftUI views thin and push logic into view models / use cases.
