# VoiceIME Agent Context

This repository is `~/code/voiceime`: a lightweight macOS 14+ SwiftPM menu-bar push-to-talk dictation app.

## Default workflow

- Inspect `git status --short --branch` before editing.
- Prefer small, focused changes and verify with `make test` before packaging.
- Build/package commands:
  - `make test`
  - `make build`
  - `make package`
  - `make install` only when explicitly asked, because it replaces `/Applications/VoiceIME.app`.
- Do not commit build products such as `.build/`, `VoiceIME.app/`, or transient logs.

## Project-specific details

- Read `README.md` for user-facing behavior, install notes, permissions, and release/download caveats.
- Read `skills/macos-swiftpm-menu-bar-voice-ime/SKILL.md` for detailed implementation guidance and historical decisions.
- Default recognition engine is Apple Speech with `zh-CN` by default.
- Local mlx-whisper fallback expects `~/.local/bin/local-transcribe`; verify it exists before relying on it.
- External LLM refinement should send transcript text only, store API keys in Keychain, and fall back to original transcript on failure.
- Input insertion uses pasteboard + simulated Cmd+V; preserve and restore pasteboard items.

## macOS permission / hotkey caveats

- Usually required: Microphone, Speech Recognition, and Accessibility.
- Input Monitoring is not required unless the code actually uses APIs that trigger it.
- Fn/Globe can be unreliable across macOS versions/keyboards. Prefer Right Option as a stable fallback/default unless the user explicitly wants deeper Fn support.
- If hotkeys fail after an upgrade, separate TCC/Accessibility failure from event matching failure; check `~/Library/Logs/VoiceIME/voiceime.log`.

## Release safety

- Current app bundles are ad-hoc signed unless changed; do not imply Developer ID signing/notarization.
- Verify package artifacts with `unzip -t dist/VoiceIME.app.zip` and `codesign --verify --deep --strict VoiceIME.app`.
- For public releases, confirm GitHub Actions/release asset status before telling the user it is available.
