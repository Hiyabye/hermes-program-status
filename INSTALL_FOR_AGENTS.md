# Install hermes-program-status with an agent

This guide is for an agent helping someone install the plugin, not for developing its source code. Explain commands in plain language. Follow the person's existing rules; this guide does not override them.

## Boundaries

- Ask for approval of the selected profiles and proposed changes before installing, enabling or disabling anything.
- Use the Hermes plugin commands below. Do not edit configuration files by hand or change identity files, instructions, credentials or unrelated settings.
- Do not install prerequisites, upgrade Hermes, use administrator privileges, delete files, or restart services. If one is needed, stop and explain why.
- Do not bypass a security warning, force an overwrite, or switch to another source when a command fails. Report the error and ask how to proceed.
- Do not post logs, profile names or other local information online. No account or API key is needed for this plugin.

## 1. Check the computer and requirements

Confirm that your command tool reaches the computer where the person runs Hermes in Herdr. A remote gateway or container may be a different computer. If you cannot establish that, ask before proceeding.

Run these read-only checks:

```sh
uname -s
hermes --version
hermes profile list
```

Stop if Hermes is missing, older than 0.23.1, or the computer is not running macOS or Linux. Explain the unmet requirement without installing or upgrading anything.

This plugin supports the classic `hermes` CLI in Herdr desktop app terminals, not `hermes --tui`. It does not install Herdr or set up the phone connection. If those are missing, explain the gap and keep this task scoped to the plugin.

## 2. Choose profiles and inspect existing plugins

Show the available profile names privately to the person and ask which ones they want. Suggest `default` if they are unsure. Do not create profiles or assume that every profile should be changed.

For each selected profile, inspect its plugins. Replace `<profile>` with its exact name, including `default` for the default profile:

```sh
hermes -p <profile> plugins list --plain --no-bundled
```

Use `-p` on every profile-specific command. Without it, the command can target the profile inherited by your shell instead of the one you intended.

Look for another enabled plugin that sends status to the Herdr desktop app, such as `herdr-osc7501`. Read its description and documentation before calling it a conflict. A launcher wrapper may also report status; if the person uses one, explain the possible overlap and ask how they want to handle it. Do not edit their launcher.

Leave `herdr-agent-state` alone. It serves Herdr TUI panes, which this plugin skips.

If `hermes-program-status` is already enabled, record that and leave it installed. If it is installed but disabled, propose enabling that copy rather than reinstalling. If its source is unclear, ask before trusting or replacing it.

## 3. Get approval, then install or enable

Describe the exact changes per profile, including any competing plugin you propose to disable. Explain that a plugin runs Python code with the person's permissions. This plugin reports status to local terminals and needs no third-party Python packages or credentials.

Wait for approval before the first change. If a competing plugin must be disabled, get approval for that specific plugin too. Leave its files in place:

```sh
hermes -p <profile> plugins disable <competing-plugin>
```

For a fresh install:

```sh
hermes -p <profile> plugins install sicruse/hermes-program-status --enable
```

For an existing, trusted copy that is disabled:

```sh
hermes -p <profile> plugins enable hermes-program-status
```

Run one profile at a time and read the full result. Stop on failure. Do not use `--force`, `--allow-removed`, dependency-consent flags or a replacement source to get past it. If the public repository or guide is unavailable, report that rather than treating this as a local-install instruction.

A successful gateway reload can load the status hooks without a restart. A new CLI session is still needed to register its terminal. If the output says a restart is required, leave it pending; restarting can interrupt other agents. The person decides when to restart.

The installer may display `after-install.md` before activation finishes. That note alone does not prove that the plugin was enabled.

## 4. Read back the state

For every selected profile, repeat:

```sh
hermes -p <profile> plugins list --plain --no-bundled
```

Confirm that `hermes-program-status` is enabled and that any competing plugin the person agreed to disable is disabled. Check the result, not just the command's exit status.

If a change failed after disabling another plugin, report that partial state. Offer to re-enable the old plugin; wait for approval before doing so.

## 5. Verify a new session in Herdr

Explain that existing CLI sessions have not registered their terminals with this plugin. Ask the person to open a new terminal in the Herdr desktop app and start the chosen profile:

```sh
hermes -p <profile>
```

Check the following with them, or inspect the new terminal if your tools can reach it:

1. The new terminal is listed as Hermes with status `idle`.
2. An ordinary prompt changes it to `working`, then `done`.
3. Asking the new session to use its clarify tool changes it to needing the person (`blocked`). Answering lets it continue and finish.
4. If they already use the Herdr phone app, the same waiting session appears under "needs you" there. Ask them to answer from the phone to check that path too.

If `hc` is available, `hc ls` can corroborate the terminal's agent name and status. Do not treat the absence of `hc` in your tool environment as proof the plugin failed.

## Completion report

List each selected profile with its install/enable result and any competing plugin disabled. State separately whether gateway activation succeeded and whether a fresh-session test passed. Never report the live behavior as verified merely because the plugin is listed as enabled.

If the person has not tested yet, say "installed and enabled; live test pending". Report other unmet prerequisites or restart requirements plainly. Do not close terminals or delete test sessions without permission.

## Undo, only if requested

Disable the plugin in a selected profile:

```sh
hermes -p <profile> plugins disable hermes-program-status
```

If the person wants to return to a competing plugin you disabled, re-enable that exact plugin only with their approval. Removing the installed folder is a separate deletion and is not part of this guide.
