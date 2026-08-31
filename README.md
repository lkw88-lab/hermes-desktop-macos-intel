# Hermes Desktop for Intel macOS (community rebuilds)

Unofficial **x86_64** builds of the
[Hermes Agent](https://github.com/NousResearch/hermes-agent) desktop app,
produced automatically when Nous Research publishes a new stable release.

The upstream project publishes desktop packages for Apple Silicon only. If
you're on an Apple Silicon Mac, use
[the official releases](https://github.com/NousResearch/hermes-agent/releases)
instead. This repository exists for Intel Mac users.

## Downloads

See [Releases](../../releases/latest). Every release contains:

| File | Purpose |
| --- | --- |
| `Hermes-<version>-mac-x64.dmg` | Drag-and-drop installer |
| `Hermes-<version>-mac-x64.zip` | Plain archive of `Hermes.app` (checksums shown per-asset on the release page) |


## Requirements

- Intel Mac (`x86_64`)
- macOS 12 (Monterey) or newer

## First launch

These builds are unsigned and unnotarized, so Gatekeeper will warn on first
run. Right-click `Hermes.app` → **Open** → **Open** again in the dialog.
If you prefer the terminal (checksums are shown per asset on the release page):

```sh
xattr -dr com.apple.quarantine /Applications/Hermes.app
```

## How it works

A scheduled GitHub Actions workflow runs daily at 00:00 UTC:

1. Compare the latest stable tag at `NousResearch/hermes-agent` with the
   releases already published here — build only when a new tag appears.
2. Check out that exact upstream tag and build the Electron desktop app for
   `x86_64` with the app's own packaging pipeline
   (`npm run dist:mac -- --x64`).
3. Verify the main executable and every packaged native binary (node-pty
   payloads) are x86_64 Mach-O, and `hdiutil verify` the DMG.
4. Publish a release with the DMG and ZIP; per-asset SHA-256 checksums are shown on the release page.

Upstream's desktop packaging scripts handle cross-arch staging via their
`before-pack` hook, so a plain `--x64` target produces correct Intel-native
binaries — no post-hoc binary surgery needed.

## Status & disclaimer

Not affiliated with or endorsed by Nous Research. No code here is vendored
from upstream — audit the app source at the official repository. Builds are
best-effort community artifacts; verify checksums before trusting them.

MIT-licensed like upstream; see [LICENSE](LICENSE).