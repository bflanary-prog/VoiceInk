# VoiceInk

Native macOS menu-bar app that transcribes speech to text locally, with AI enhancement and app-aware "Power Mode". GPLv3, upstream by Beingpax.

## Stack
- Swift / SwiftUI, AppKit, SwiftData, AppIntents. macOS 14.4+ target (some targets 15.0).
- Local transcription via whisper.cpp (`whisper.xcframework`) and FluidAudio (Parakeet).
- SwiftPM deps: Sparkle, KeyboardShortcuts/KeySender, LaunchAtLogin-Modern, Zip, SelectedTextKit, AXSwift, swift-atomics, mediaremote-adapter, LLMkit.

## Run / test
- `make all` — check prerequisites + build (clones/builds whisper.xcframework into `~/VoiceInk-Dependencies` first run).
- `make dev` — build and run; `make run` launches the built app.
- `make local` — ad-hoc-signed build (no Apple Developer cert) → `~/Downloads/VoiceInk.app`.
- Tests: run the `VoiceInkTests` / `VoiceInkUITests` schemes in Xcode (Cmd+U), or `xcodebuild test`.

## Layout
- `VoiceInk/` — app source (entry `VoiceInk.swift`, `AppDelegate.swift`).
- `VoiceInk/Transcription/` — engines: `Whisper`, `FluidAudio`, `Native`, `Cloud`, `Streaming`, `Processing`.
- `VoiceInk/Services/` — managers (AI enhancement, Ollama, license/Polar, Keychain, audio devices).
- `VoiceInk/Models/` — SwiftData models, prompts, model registry.
- `VoiceInk/PowerMode/`, `VoiceInk/Views/`, `VoiceInk/Shortcuts/`, `VoiceInk/AppIntents/`.
- `VoiceInk.xcodeproj/` — project; `Makefile` / `BUILDING.md` — build automation.

## Conventions / gotchas
- whisper.xcframework is not vendored; the Makefile builds it. `make clean` removes deps + `.local-build`.
- Local builds use `LocalBuild.xcconfig` + `VoiceInk.local.entitlements` + `LOCAL_BUILD` flag; normal builds unaffected.
- Secrets/API keys stored via macOS Keychain (`KeychainService.swift`), not files.
- Upstream is NOT accepting PRs; fork-only. Sparkle auto-update wired via `appcast.xml` / `announcements.json`.
