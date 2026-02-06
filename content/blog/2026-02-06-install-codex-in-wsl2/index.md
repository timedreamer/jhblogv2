---
title: Fix OpenAI Codex Login on WSL2 (Debian)
date: '2026-02-06'
categories:
    - tools
tags:
    - codex
    - wsl2
    - debian
---

I recently set up the OpenAI Codex CLI on my Windows 10 machine using WSL2 (Debian). While the installation went smoothly, I hit a networking wall when trying to authenticate. Here is the quick fix.

### The Problem
After running `codex login`, the CLI attempts to open a browser window to authenticate. By default, this opens Chrome in Windows. 

However, Windows cannot always see the local server running inside the Linux VM. The browser fails with:

> **This site can’t be reached**
> localhost refused to connect.

### The Solution
Since the Windows browser can't reach the WSL port, the solution is to use a browser running *inside* WSL.

**1. Install Firefox ESR**
Since I am using Debian, the package is `firefox-esr` rather than standard Firefox.

```bash
sudo apt update
sudo apt install firefox-esr -y
```

**2. Launch Firefox in the Background**
Open the Linux browser from your terminal.

```bash
firefox-esr &
```

**3. Authenticate**
Run the login command again:

```bash
codex login
```

If the link still opens in Windows Chrome, copy the URL from the address bar and paste it into the **Linux Firefox** window you just opened. The internal connection will succeed, and the CLI will save your credentials.

