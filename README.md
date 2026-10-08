# gray-rules

Inject rule files into the session before the agent's first request. Port of
pi's `claude-rules` extension.

A sidecar plugin for [gray](https://github.com/vstaln/gray), scaffolded by
[gray-account](https://github.com/vstaln/gray-account).

## What it does

On `agent/before_start` (protocol 2.0) the sidecar reads, in order, every
file that exists:

1. `~/.claude/CLAUDE.md`
2. `<session.cwd>/CLAUDE.md`
3. `<session.cwd>/AGENTS.md`

and returns their concatenated contents — per-file `## path` headers, 16 KiB
total cap — as the pre-turn user message. Nothing found → `{}` (no message).

`/rules` lists which files would be injected for the current session cwd,
with byte sizes and a truncation note when the cap would kick in.

## Wire methods

`plugin/manifest` · `agent/before_start` (hook) · `command/run` (`/rules`) ·
`plugin/shutdown`

## Install

```sh
gray plugin install rules
```

## Develop

```sh
cargo test
gray account check      # entry point + manifest handshake
gray account publish    # check → build → release → publish to the gray registry
```

Bump `version` in `Cargo.toml` before each `publish`; the registry refuses to
republish a version.
