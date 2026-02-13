---
title: Fixing tmux 3.4+ OSC 11 escape leak
author: Ji Huang
date: "2026-02-13"
categories:
    - misc
tags:
    - tmux
    - bash
    - terminal
    - escape-sequences
description: 'Stop tmux 3.4+ OSC 10/11 terminal color responses from leaking into bash (garbled prompt / "11: command not found").'
---

When I upgraded to **tmux 3.4+**, new sessions sometimes started with garbage characters at the prompt, followed by bash errors.

Tested with **Windows Terminal Version: 1.23.20211.0** (tmux running in a shell inside that terminal).

## Symptom

You may see something like this right after tmux starts:

```text
^[]11;rgb:2828/2c2c/3434^[\
11: command not found
-bash: rgb:2828/2c2c/3434: No such file or directory
```

This is your terminal replying to an **OSC 11** query (background color). The reply sometimes arrives “late” and gets forwarded into the first pane’s PTY, so bash reads it as input and tries to execute it.

## Quick fix (bash + tmux)

The workaround I use is:

1. Turn off echo early in `.bashrc` (so the garbage isn’t printed).
2. Drain any pending bytes at the end of `.bashrc` (so bash doesn’t execute them).

### 1) Suppress echo early (top of `~/.bashrc`)

Put this near the top, right after the interactive-shell check:

```bash
# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return ;;
esac

# tmux 3.4+ may query the outer terminal via OSC 10/11; the response can
# leak into the first pane input. Disable echo early to avoid showing it.
if [ -n "${TMUX:-}" ]; then
    stty -echo
fi
```

### 2) Drain and restore (bottom of `~/.bashrc`)

Put this at the very bottom:

```bash
# Drain stray terminal responses so they don't get executed as commands.
if [ -n "${TMUX:-}" ]; then
    while IFS= read -r -s -t 0.05 -n 1; do :; done
    stty echo
fi
```

## Notes

- This is a workaround for a timing/race issue: tmux probes terminal features at startup, and replies can arrive after the shell is already running.
- If your `.bashrc` is heavy, you might notice a tiny delay (the final `read -t 0.05` timeout).
- If you use `zsh`/`fish`, adapt the same idea: disable echo ASAP when `$TMUX` is set, then drain input and restore echo at the end.
- This solution works on **Windows Terminal (1.23.20211.0)** for me.

## References

- tmux issue tracker reports around OSC 10/11 response leaks and duplicates:
    - [tmux/tmux#3838](https://github.com/tmux/tmux/issues/3838)
    - [tmux/tmux#3470](https://github.com/tmux/tmux/issues/3470)
    - [tmux/tmux#4634](https://github.com/tmux/tmux/issues/4634)
- Theme-level reports:
    - [gpakosz/.tmux#720](https://github.com/gpakosz/.tmux/issues/720)
