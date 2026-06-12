---
title: "Fix Codex CLI login on a remote Ubuntu server using WSL2 auth.json"
date: 2026-06-08
draft: false
tags: ["Codex", "Ubuntu", "WSL2", "SSH"]
---

I use Codex CLI on an Ubuntu server. After updating Codex, the old `service_tier = "default"` config error went away, but I still could not finish browser login from the server.

The problem was the OAuth callback URL:

```text
http://localhost:1455/auth/callback?code=...
```

Codex was running on the remote server, but the browser was running on my Windows machine. In the browser, `localhost` means Windows, not the remote Ubuntu server. So the browser could not reach the Codex callback listener on the server.

## What I tried

First, I tried the usual SSH local forwarding method:

```powershell
ssh -L 1455:localhost:1455 jhuang@galaxy3
```

Then I tried binding a different local port:

```powershell
ssh -N -o ExitOnForwardFailure=yes -L 127.0.0.1:49152:127.0.0.1:1455 jhuang@galaxy3
```

This partially worked, but it was still unreliable on my Windows setup. I also tried using Firefox on the server:

```bash
firefox http://localhost:1455/
```

That failed because the server SSH session had no GUI display:

```text
Error: no DISPLAY environment variable specified
```

Device-code login was not available because it was disabled by my admin. API-key auth was also not an option.

## The working fix

I already had Codex installed and logged in inside WSL2. So I copied the WSL2 Codex auth file to the remote server.

In WSL2, the file was here:

```bash
/home/td/.codex/auth.json
```

I created the same file on the remote server:

```bash
mkdir -p ~/.codex
nano ~/.codex/auth.json
```

Then I copied the full content of the WSL2 `auth.json` into the server file and fixed the permissions:

```bash
chmod 700 ~/.codex
chmod 600 ~/.codex/auth.json
```

After that, Codex worked on the remote server:

```bash
codex
```

## Why this works

Codex stores login state under `~/.codex`. The browser login had already succeeded in WSL2, so WSL2 had a valid `auth.json`. Copying that file to the remote server gave the server-side Codex CLI the same login state.

This avoids all three blocked paths:

- no remote browser is needed;
- no SSH callback tunnel is needed;
- no device-code or API-key login is needed.

## Notes

Treat `auth.json` like a password. Do not paste it into chat, commit it to Git, or store it in a shared folder.

If the login stops working later, repeat the login in WSL2 and copy the refreshed `auth.json` to the server again.
