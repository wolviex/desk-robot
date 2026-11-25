# Project scope

## What this is

desk-robot ("Rocky") is a small desk robot with a face, a voice, a camera and a pan-tilt neck. You say "hey Rocky",
and it turns, looks at you and answers out loud in character. It's an open-source build guide plus the code for it.

There are two parts:

| Part | Where | Does |
|---|---|---|
| Body (firmware) | `firmware/` (XIAO ESP32S3 Sense, PlatformIO) | Draws the face, moves the servos, streams mic audio and camera frames, plays speech. Kept deliberately simple. |
| Brain (server) | `server/` (Python, on the user's computer) | Wake word, speech-to-text, language model, text-to-speech, face tracking, emotions, abilities, dashboard. All the judgement lives here. |

They talk over WiFi (WebSocket, port 8765, shared token). The brain serves a local page and control API on
`127.0.0.1:8767` (`POST /api/<action>`). The launcher (`server/launcher.py`) serves the dashboard on 8766.

## In scope

- The V2 robot: hardware build docs (`README.md`, `docs/wiring.md`), the firmware and the brain.
- Features driven by voice and the dashboard: conversation, vision, focus mode, "tell me when", drawing, Pictionary,
  dances and computer control.
- Making it the user's own: characters, voices, faces, models and providers.
- Integrations with other systems through the brain's existing seams: the OpenAI-compatible LLM endpoint
  (`LLM_BASE_URL`) and the local control API. OpenClaw is the current candidate (see `PROJECT_STATE.md`).
- Tests under `server/tests/` and the changelog and release notes for each version.

## Out of scope

- V1. It's frozen as the `v1.0` release and is never changed.
- Cloud hosting of the brain. It runs on the user's own computer, and nothing is billed while idle.
- Exposing the control API or camera beyond this computer by default (`LIVE_VIEW_BIND` stays `127.0.0.1`).
- Recreating copyrighted characters' assets. Rocky's character is the user's to change, and none is shipped
  besides the text persona.

## Constraints

- Firmware stays thin. New behaviour goes in the brain unless it physically can't.
- The privacy defaults in the README's "Privacy and security" section hold. Any change that sends more data
  off the machine says so in the README and the changelog.
- Python dependencies are pinned in `server/requirements.txt`. Adding one is a deliberate decision noted in the
  changelog.
- Each release is a git tag plus a `CHANGELOG.md` entry that says what to reflash and what to reinstall.
