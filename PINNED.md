# Digital Fennec hardened fork

- Upstream: https://github.com/lemomo-ai/lemo-opuscar (MIT code; demo films and stills CC BY 4.0).
- Scanned and pinned base: `b5e8ef93930b6d828f352055a689320f138237b1` (upstream main on 2026-10-22, Bastion review R-261022-02). Upstream main had no newer commit on 2026-10-22.
- Branch `ayla-pinned` = that commit + our hardening commits only.

## What we changed
1. No auto-update. `setup.sh` no longer fetches upstream main and force-resets the clone to it. It clones our branch once and never updates.
2. No upstream plugin or skill text. Removed `.claude-plugin/`, `plugin/.claude-plugin/`, `plugin/skills/lemo-opuscar/SKILL.md` and the plugin-directory builder (`tools/directory*`). Our agents use the Ayla skill `ayla-video-styles` (our own text).
3. No online voice. Removed `core/tts/tts_zh.py` and `edge-tts` (it sent narration text to Microsoft). Voices are Kokoro (EN, FR) on this machine.
4. Python packages pinned with sha256 hashes (`requirements*.txt`, `--require-hashes`). npm uses `npm ci --ignore-scripts` on the existing lockfile.
5. Removed the GitHub Pages workflow (`.github/`).

## Network use that is left (no client content is sent)
- `tools/fetch.sh`: downloads asset packs (voice model, HDRIs, instrument samples) from the upstream "assets" release. Each pack is checked against the sha256 in `tools/assets.json` before it is unpacked.
- `playwright-core install chromium-headless-shell`: the headless browser binary.
- `core/tts/asr_check.py`: downloads the faster-whisper model from Hugging Face once. Speech check runs locally.

## Updating from upstream
Manual only: diff upstream against this branch, re-scan every new commit (Bastion), new GO from Hichem, then merge into `ayla-pinned`.
