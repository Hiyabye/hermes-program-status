# Contributing

Thanks for helping. This is a small plugin, and the aim is to keep it small, readable and safe to run inside every Hermes session.

## Ground rules

- Public plugin surfaces only. Use documented Hermes hooks and public modules. No monkey-patching, no import hooks, no wrapping of Hermes internals. `hermes plugins validate .` must pass, including the "no core override" check.
- Never block or break the agent. Every write is one small non-blocking call, and every failure is silent.
- No network access, no subprocesses, no credentials, no telemetry.
- Standard library only. No third-party dependencies.

## Reporting a bug

Open an issue with:

- Hermes version (`hermes --version`), operating system, and how you start Hermes (classic CLI, profile, gateway on or off).
- Your terminal (Herdr desktop app version and `hc --version`, or another terminal).
- What you expected and what `hc get <terminal> --json` showed.
- If you can, a trace: set `HERMES_PROGRAM_STATUS_LOG=/tmp/hps.jsonl` before starting Hermes and attach the lines around the problem. Check it for anything private before you post it.

## Making a change

1. Fork the repository and create a branch.
2. Make the change, with a test in `tests/` for any behavior change.
3. Run the checks:

   ```sh
   python3 -m unittest discover -s tests
   hermes plugins validate .
   ```

4. If you changed the hooks the plugin registers, update `provides_hooks` in `plugin.yaml` to match. A test checks this.
5. Add a line to `CHANGELOG.md` under "Unreleased".
6. Open a pull request that says what changed and how you tested it against a real Hermes session.

## Style

- Python 3.11 or newer, with `from __future__ import annotations`.
- Keep the plugin in one file unless there is a strong reason not to.
- Plain, short comments that explain why, not what.

## Code of conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md). By taking part you agree to it.
