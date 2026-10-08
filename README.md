<p align="center">
  <img src="assets/gray-logo.svg" alt="gray" width="96">
</p>
<h1 align="center">gray-rules</h1>
<p align="center">Inject repository rule files before the first agent request.</p>
<p align="center">
  <a href="https://github.com/vstaln/gray-rules/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
  <img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-7aa2f7.svg">
  <img alt="rust" src="https://img.shields.io/badge/built%20with-rust-orange.svg">
</p>

Load local rule files into the session before the agent's first request.

## What it does

On `agent/before_start` (protocol 2.0), the sidecar reads every file that
exists, in order:

1. `~/.claude/CLAUDE.md`
2. `<session.cwd>/CLAUDE.md`
3. `<session.cwd>/AGENTS.md`

Their concatenated contents — with per-file `## path` headers and a 16 KiB
total cap — are returned as the pre-turn user message. When nothing is
found, the hook returns `{}` and injects no message.

`/rules` lists which files would be injected for the current session cwd,
with byte sizes and a truncation note when the cap would apply.

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

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
