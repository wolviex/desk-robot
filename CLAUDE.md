# CLAUDE.md

@.claude/multi-agent.md

@PROJECT_SCOPE.md

@PROJECT_STATE.md

## Project files

- `PROJECT_SCOPE.md`: what this project is and is not. Change it only when the user changes the scope.
- `PROJECT_STATE.md`: where the work stands. The orchestrator updates it after each accepted task and at sign-off,
  in the same commit as the work it describes.

## Required checks

- Server tests, for any change under `server/`: `cd server && python3 -m pytest -q -p no:warnings`
  (needs `pip install -r server/requirements.txt`). Expected baseline: everything passes except the two
  `tests/test_perception.py::WaveTests` tests where `libEGL.so.1` is missing (headless Linux containers). Those two
  count as environmental, not as failures, only when their error is that missing library.
- Firmware, for any change under `firmware/`: `cd firmware && pio run` (PlatformIO). If `pio` is not installed, the
  check could not run: report it as BLOCKED, never as a pass.
- Docs-only changes (`README.md`, `docs/`, `CHANGELOG.md`): no automated check; the supervisor reviews the text.
