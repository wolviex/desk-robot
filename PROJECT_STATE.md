# Project state

Last updated: 2026-11-25

## Current version

- Software v2.2 (2026-11-24): "tell me when", drawing and Pictionary.
- Firmware 0.4.4. Drawing and Pictionary need it; everything else works on 0.4.3.

## Workflow

- Multi-agent framework installed (2026-11-25) from `wolviex/Claude-Multi-Agent`: `.claude/multi-agent.md`, the
  `implementer`, `supervisor` and `architect` agents, the `block-git-write` and `agent-gate` hooks, and the
  `multiAgent` roles in `.claude/settings.json` (orchestrator opus, supervisor sonnet, implementer haiku,
  architect fable). The architect writes to `dev/`.
- Required checks are listed in `CLAUDE.md`.

## Check baseline (2026-11-25, Linux container, Python 3.13)

- `server` tests: 181 passed, 2 failed. The failures are `test_perception.py::WaveTests` (2 tests), and both are
  environmental: mediapipe needs `libEGL.so.1`, which the container doesn't have.
- Firmware build: not run here (PlatformIO isn't installed in the container).

## In progress / open decisions

- **OpenClaw integration.** It's been explored but not started. There are two options:
  1. Rocky as OpenClaw's voice: point `LLM_BASE_URL` at the OpenClaw Gateway's OpenAI-compatible
     `/v1/chat/completions` (`gateway.http.endpoints.chatCompletions.enabled: true`, model `openclaw/default`).
     These are unverified: whether the Gateway honours the client `tools` Rocky sends (he needs them to move his
     head and use abilities), whether it accepts images, and how much latency it adds.
  2. OpenClaw controlling Rocky: an OpenClaw skill calling the brain's `POST /api/<action>`. This needs new `say`
     and `ask` actions, which today exist only as console commands (`server/brain/main.py:347`).

  The recommendation is to start with option 2. It's waiting on the user's choice.

## Next steps

- [ ] User picks an OpenClaw option.
- [ ] Commit the multi-agent framework and these project files to the session branch.
