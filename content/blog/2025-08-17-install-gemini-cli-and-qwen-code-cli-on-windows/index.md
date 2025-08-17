---
title: Install Gemini-cli and Qwen-code on Windows
author: Ji Huang
date: '2025-08-17'
slug: []
categories:
  - dry lab
tags:
  - AI
meta_img: images/image.png
description: Description for the page
---

- [gemini-cli](https://github.com/google-gemini/gemini-cli)
- [qwen-code](https://github.com/QwenLM/qwen-code)

Tested on Windows 10 with Powershell.

## 1. Install Node.js

Open PowerShell and run:

```powershell
winget install -e --id OpenJS.NodeJS
```

Verify installation:

```powershell
node -v
npm -v
```

### If `node` is not recognized

Add Node.js to the PATH:

**Double check `node.exe` is in `C:\Program Files\nodejs` folder.**

```powershell
[Environment]::SetEnvironmentVariable(
  "Path",
  ([Environment]::GetEnvironmentVariable("Path","User") + ";C:\Program Files\nodejs"),
  "User"
)
$env:Path += ";C:\Program Files\nodejs"
```

Check again:

```powershell
node -v
npm -v
```

---

## 2. Install Gemini CLI

Install globally with npm:

```powershell
npm install -g @google/gemini-cli
```

Run it:

```powershell
gemini
```

Authenticate in the browser when prompted.

---

## 3. Install Qwen-Code

Install globally with npm:

```powershell
npm install -g @qwen-code/qwen-code@latest
```

Check installation:

```powershell
qwen --version
```

Run it:

```powershell
qwen
```

Sign in with your Qwen account (or configure with API key).


