---
title: Migrate blogdown site to Hugo + PaperMod (no R)
author: Ji Huang
date: "2026-02-13"
categories:
    - misc
tags:
    - hugo
    - blog
    - papermod
    - netlify
    - wsl
description: A step-by-step record of how I migrated this repo (timedreamer/jhblogv2) from blogdown to pure Hugo + PaperMod.
---


This post records what I did to migrate my blog repo `timedreamer/jhblogv2` from an **R/blogdown**-centric setup to a **pure Hugo** workflow (Markdown + VS Code), using the **PaperMod** theme:

- Repo: [https://github.com/timedreamer/jhblogv2](https://github.com/timedreamer/jhblogv2)
- Theme: [https://github.com/adityatelange/hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod)

The final blog is at: [https://jhuang.netlify.app/](https://jhuang.netlify.app/)

## 0) Target workflow (after migration)

- Write posts in Markdown in VS Code.
- Preview locally in WSL2 with `hugo server`.
- Commit + push to GitHub.
- Netlify builds and deploys.

## 1) Install/verify Hugo (WSL2)

PaperMod requires a modern Hugo. On WSL2 (Debian), I installed Hugo from the **official GitHub release** (instead of the old bookworm `apt` version).

### Install from GitHub release (`.deb`)

```bash
# optional: remove the old Debian repo version first
sudo apt-get remove -y hugo

# install a recent Hugo extended build from GitHub Releases
HUGO_VER=0.155.3
wget -O /tmp/hugo_extended.deb "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VER}/hugo_extended_${HUGO_VER}_linux-amd64.deb"
sudo dpkg -i /tmp/hugo_extended.deb
```

### Verify

```bash
which hugo
hugo version
```

On my machine at the time of this migration, `hugo version` showed `v0.155.2` (so anything `>= 0.146.0` is fine for PaperMod).

## 2) Add PaperMod as a git submodule

I added PaperMod under `themes/PaperMod`:

```bash
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

Netlify also needs submodules enabled; I set:

- `GIT_SUBMODULE_STRATEGY = "recursive"` in `netlify.toml`

## 3) Update Hugo config for PaperMod

In `config.yaml`, I switched the theme and set basic PaperMod params:

1. Set theme to PaperMod (`theme: [PaperMod]`).
2. Keep `markup.goldmark.renderer.unsafe: true` (some posts include HTML).
3. Keep the `/blog/` section as the main list (`mainSections: ["blog"]`).
4. Configure the top menu (Home/About/Twitter/Subscribe).

## 4) Remove R/blogdown files

I deleted the files that made this repo blogdown/R-centric:

- `.Rprofile`
- `index.Rmd`
- `*.Rproj`
- `R/`

I also removed the blogdown shortcode:

- `layouts/shortcodes/blogdown/postref.html`

## 5) Keep legacy R posts without R

This repo had two old `.Rmd` posts that were already rendered to HTML.
To avoid needing R forever, I kept the rendered HTML and removed the `.Rmd` sources:

- Keep `index.html` and `index_files/`
- Delete `index.Rmd`
- Replace any `blogdown/postref`-style paths in the HTML with plain relative paths like `index_files/...`

## 6) Local testing (WSL2)

I hit a permissions issue with Hugo cache under `~/.cache`, so I used a project-local cache dir.

1. Production-like build:

    ```bash
    mkdir -p .cache/hugo
    HUGO_CACHEDIR=$(pwd)/.cache/hugo hugo --gc --minify
    ```

2. Local preview server:

    ```bash
    HUGO_CACHEDIR=$(pwd)/.cache/hugo hugo server -D --disableFastRender
    ```

3. Open in Firefox:

    - `http://localhost:1313/`
    - `http://localhost:1313/blog/`
    - A few older posts and image-heavy posts

## 7) Netlify deployment

PaperMod needs modern Hugo, so I pinned Hugo in `netlify.toml`:

- `HUGO_VERSION = "0.146.0"`
- `GIT_SUBMODULE_STRATEGY = "recursive"`

2. Push to GitHub, then verify Netlify deploy preview before merging to `main`.

## 8) Writing new posts (pure Markdown)

I now write posts as pure Markdown bundles:

    ```bash
    mkdir -p content/blog/2026-02-13-my-title
    code content/blog/2026-02-13-my-title/index.md
    ```

2. Put images next to `index.md` and reference them relatively:

- `![caption](my-image.png)`

3. Preview with `hugo server -D`.
