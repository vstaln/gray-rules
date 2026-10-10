<div align="center">
  <img alt="gray-rules" src="assets/icon.svg" width="120" height="120" />
  <h1>gray-rules</h1>
  <p><strong>Load local rule files into the session before the first agent request.</strong></p>
  <p>
    <a href="https://gray.alignment.id">Website</a> ·
    <a href="https://gray.alignment.id/plugins/gray-rules">Store</a> ·
    <a href="https://github.com/vstaln/gray-rules">Source</a> ·
    <a href="https://github.com/vstaln/gray">gray</a>
  </p>
  <p>
    <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-1c1c20?style=flat-square&labelColor=0a0a0b" /></a>
    <a href="https://www.rust-lang.org"><img alt="Built with Rust" src="https://img.shields.io/badge/built%20with-rust-1c1c20?style=flat-square&labelColor=0a0a0b&logo=rust&logoColor=d4a373" /></a>
    <a href="https://gray.alignment.id/plugins/gray-rules"><img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-1c1c20?style=flat-square&labelColor=0a0a0b&color=7aa2f7" /></a>
  </p>
</div>

<br/>

```bash
gray plugin install gray-rules
```

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

## Tags

`gray` `plugin` `rules` `rust`

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
