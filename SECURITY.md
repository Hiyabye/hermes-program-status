# Security policy

## Reporting a problem

Please do not open a public issue for a security problem. Email si@sicruse.com with a description, the steps to reproduce it, and the Hermes and terminal versions involved. You should get a reply within five working days.

## What the plugin can touch

The plugin runs inside Hermes with your user's permissions. By design it:

- reads Hermes's terminal-session files under `<HERMES_HOME>/terminal-sessions/` (default home and profile homes),
- writes and removes small JSON files under `<HERMES_HOME>/plugin-data/hermes-program-status/`,
- writes short OSC 7501 escape sequences to tty devices registered by a live Hermes CLI of the same user.

It makes no network connections, starts no processes, and reads no credentials. Anything that goes beyond that list is a bug worth reporting.

## Supported versions

Only the latest release receives fixes.
