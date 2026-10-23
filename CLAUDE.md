# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A single-file [herdr](https://github.com/herdrdev/herdr) configuration (`config.toml`) checked
out **directly into `~/.config/herdr`**. The repo root *is* the live config directory, so editing
`config.toml` here edits the running config. There is no install step and no symlink.

The config mirrors the [tmux setup](https://github.com/anwarahmed/tmux): a tmux session becomes a
herdr workspace, a window becomes a tab, and a pane stays a pane. Most comments in `config.toml`
name the tmux setting a herdr option stands in for. Keep that pairing when adding options, and
check `~/.config/tmux/tmux.conf` when you're unsure what a binding should be.

## Commands

```sh
herdr config check             # validate config.toml and print diagnostics
herdr server reload-config     # apply it to the running server (also prefix+q)
omarchy-restart-herdr          # the same reload, through Omarchy, and only if a server is running
herdr --default-config         # every option and action with its default, commented out
omarchy-menu-herdr-keybindings --print   # the resolved bindings, as Omarchy's menu shows them
```

## The Omarchy relationship

Omarchy ships a default at `/usr/share/omarchy/config/herdr/config.toml`. As of the initial
commit (2026-10-20), this repo's `config.toml` is that file plus one line, `onboarding = false`.

- The migration that installed herdr only **seeds** the config if none exists. It does not
  touch an existing file.
- `omarchy-refresh-herdr` **overwrites** `config.toml` with the Omarchy default (the previous
  copy is left as `config.toml.bak.<epoch>`, which is gitignored) and reloads. The
  `onboarding = false` line, and any other local change, is lost and must be re-applied.
- `omarchy-menu-herdr-keybindings` (the `prefix+?` help) builds its list from
  `herdr --default-config` overlaid with this file. A binding is listed whether or not it is
  commented here.

To see what's personal, diff against the Omarchy default.

## Runtime files in this directory

herdr writes its state next to the config: `*.log`, `*.sock`, `herdr.sock.agent`,
`session.json`, `sessions/`, `session-snapshots/`, `release-notes.json` and `.plugins.lock`.
They are gitignored. Never commit them, and don't delete the sockets while a server is running.
