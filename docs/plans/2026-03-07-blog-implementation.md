# Offensive Programming Blog — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Set up a personal Hugo blog with the Blowfish theme, deployed to GitHub Pages via GitHub Actions.

**Architecture:** Hugo static site generator with the Blowfish theme (git submodule). Content written in markdown, stored in git. GitHub Actions builds on push to main and deploys to GitHub Pages.

**Tech Stack:** Hugo (extended), Blowfish theme, GitHub Actions, GitHub Pages

**Design doc:** `docs/plans/2026-03-07-blog-design.md`

---

## Task 1: Install Hugo

**Context:** Hugo is not currently installed. Go is not installed either but is not required for basic Hugo usage — Hugo ships as a single binary. We need the "extended" edition for Sass/image processing support (required by Blowfish's Tailwind CSS).

**Step 1: Install Hugo Extended via winget**

Run:
```bash
winget install -e --id Hugo.Hugo.Extended
```

Expected: Hugo Extended is installed and added to PATH.

**Step 2: Verify installation**

Close and reopen your terminal (so PATH updates), then run:
```bash
hugo version
```

Expected: Output shows `hugo v0.1xx.x+extended` (the `+extended` part is important).

---

## Task 2: Initialize Hugo Site

**Files:**
- Create: `hugo.toml` (will be replaced in Task 3)
- Create: `archetypes/default.md`

**Step 1: Create a new Hugo site in the current directory**

Run from `C:\Users\Mistress\claude_projects\blog`:
```bash
hugo new site . --force
```

The `--force` flag is needed because the directory already has files (our docs/ folder).

Expected: Hugo scaffolds the site structure:
```
archetypes/
assets/
content/
data/
i18n/
layouts/
static/
themes/
hugo.toml
```

**Step 2: Initialize git repo**

```bash
git init
```

Expected: `.git/` directory created.

**Step 3: Commit scaffold**

```bash
git add -A
git commit -m "chore: initialize Hugo site scaffold

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

## Task 3: Install Blowfish Theme

**Files:**
- Create: `themes/blowfish/` (git submodule)
- Create: `config/_default/hugo.toml`
- Create: `config/_default/languages.en.toml`
- Create: `config/_default/markup.toml`
- Create: `config/_default/menus.en.toml`
- Create: `config/_default/params.toml`
- Delete: `hugo.toml` (root-level, replaced by config/_default/)

**Step 1: Add Blowfish as git submodule**

```bash
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

Expected: `themes/blowfish/` populated with theme files, `.gitmodules` file created.

**Step 2: Copy default config files from theme**

```bash
mkdir -p config/_default
cp themes/blowfish/config/_default/*.toml config/_default/
```

Expected: 5 config files in `config/_default/`.

**Step 3: Remove root hugo.toml (config now lives in config/_default/)**

```bash
rm hugo.toml
```

**Step 4: Configure `config/_default/hugo.toml`**

Replace contents with:
```toml
theme = "blowfish"
baseURL = "https://USERNAME.github.io/"
defaultContentLanguage = "en"
enableRobotsTXT = true

[pagination]
  pagerSize = 10

[outputs]
  home = ["HTML", "RSS", "JSON"]

[markup]
  [markup.highlight]
    noClasses = false
```

> **Note:** Replace `USERNAME` with your actual GitHub username. We'll set this after creating the repo.

**Step 5: Configure `config/_default/languages.en.toml`**

Replace contents with:
```toml
languageCode = "en"
languageName = "English"
weight = 1
title = "Offensive Programming Blog"

[params]
  [params.author]
    name = "Your Name"
    bio = "Staff Developer. Writing about code, systems, and everything in between."
    # image = "img/author.jpg"  # Uncomment and add your photo to assets/img/
```

> **Note:** Update `name` and `bio` with your real info.

**Step 6: Configure `config/_default/params.toml`**

Key settings to change from defaults:
```toml
colorScheme = "blowfish"
defaultAppearance = "dark"
autoSwitchAppearance = true

enableSearch = true
enableCodeCopy = true

[article]
  showDate = true
  showAuthor = true
  showReadingTime = true
  showTableOfContents = true
  showPagination = true
```

> Leave other params.toml settings at their defaults — Blowfish's defaults are sensible.

**Step 7: Configure `config/_default/menus.en.toml`**

Replace contents with:
```toml
[[main]]
  name = "Posts"
  pageRef = "posts"
  weight = 10

[[main]]
  name = "About"
  pageRef = "about"
  weight = 20
```

**Step 8: Verify the site builds**

```bash
hugo server
```

Expected: Site builds and serves locally at `http://localhost:1313/`. You should see the Blowfish theme with "Offensive Programming Blog" as the title. Press Ctrl+C to stop.

**Step 9: Commit theme setup**

```bash
git add -A
git commit -m "feat: add Blowfish theme with site configuration

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

## Task 4: Create Content Pages

**Files:**
- Create: `content/about.md`
- Create: `content/posts/_index.md`
- Create: `content/posts/hello-world/index.md`

**Step 1: Create the About page**

Create `content/about.md`:
```markdown
---
title: "About"
description: "About Offensive Programming Blog"
showDate: false
showReadingTime: false
showAuthor: true
---

Welcome to Offensive Programming Blog.

I'm a Staff Developer writing about code, systems, and the craft of software engineering.

This blog is forever free and open. No paywalls, no subscriptions — just content.
```

**Step 2: Create the posts listing page**

Create `content/posts/_index.md`:
```markdown
---
title: "Posts"
description: "All blog posts"
---
```

**Step 3: Create a "Hello World" post with mermaid demo**

Create `content/posts/hello-world/index.md`:
```markdown
---
title: "Hello World"
date: 2026-03-07
description: "First post — testing markdown, code, and mermaid diagrams."
tags: ["meta"]
---

Welcome to Offensive Programming Blog. This is the first post — a quick test of the features this blog supports.

## Code Blocks

Here's some Go, because why not:

` + "```" + `go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Offensive Programming Blog!")
}
` + "```" + `

## Mermaid Diagrams

Here's a simple architecture diagram:

{{< mermaid >}}
graph LR
    A[Write Markdown] --> B[Git Push]
    B --> C[GitHub Actions]
    C --> D[Hugo Build]
    D --> E[GitHub Pages]
    E --> F[Readers]
{{< /mermaid >}}

## What's Next

More posts coming soon. Topics will include:

- Systems design and architecture
- Code deep-dives
- Engineering culture and practices
- Tools and workflows
```

**Step 4: Verify content renders**

```bash
hugo server
```

Expected:
- Homepage shows the "Hello World" post
- Navigate to `/posts/` — post is listed
- Navigate to `/posts/hello-world/` — full post with code highlighting and mermaid diagram
- Navigate to `/about/` — about page renders
- Press Ctrl+C to stop

**Step 5: Commit content**

```bash
git add -A
git commit -m "feat: add about page and hello-world post with mermaid demo

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

## Task 5: Add GitHub Actions Deploy Workflow

**Files:**
- Create: `.github/workflows/hugo.yaml`

**Step 1: Create the workflow file**

Create `.github/workflows/hugo.yaml`:
```yaml
name: Build and deploy

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.156.0
    steps:
      - name: Checkout
        uses: actions/checkout@v6
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Install Hugo
        run: |
          curl -sLJO "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          mkdir -p "${HOME}/.local/hugo"
          tar -C "${HOME}/.local/hugo" -xf "hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          rm "hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          echo "${HOME}/.local/hugo" >> "${GITHUB_PATH}"

      - name: Build the site
        run: |
          hugo build \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**Step 2: Create `.gitignore`**

Create `.gitignore` in repo root:
```
# Hugo build output
public/
resources/_gen/

# OS files
.DS_Store
Thumbs.db

# Editor files
*.swp
*.swo
*~
.idea/
.vscode/
```

**Step 3: Commit workflow**

```bash
git add -A
git commit -m "ci: add GitHub Actions workflow for Hugo deployment

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

## Task 6: Create GitHub Repo and Deploy

**Prerequisites:** You need the `gh` CLI installed, or do this manually on github.com.

**Step 1: Create a GitHub repository**

```bash
gh repo create USERNAME.github.io --public --source=. --push
```

> Replace `USERNAME` with your GitHub username. For a user site (username.github.io), the repo name must match exactly.

> **Alternative:** If you prefer a project site (e.g., `USERNAME.github.io/blog`), name the repo `blog` instead and adjust baseURL accordingly.

**Step 2: Enable GitHub Pages in repo settings**

Go to: `https://github.com/USERNAME/USERNAME.github.io/settings/pages`

- Source: **GitHub Actions** (not "Deploy from a branch")

**Step 3: Push and verify deployment**

```bash
git push -u origin main
```

Expected:
- GitHub Actions workflow triggers automatically
- After ~1-2 minutes, site is live at `https://USERNAME.github.io/`
- Verify the hello-world post, about page, mermaid diagram, and code highlighting all work

**Step 4: Commit any baseURL fixes if needed**

If the URL is different from what you configured in `hugo.toml`, update `baseURL` and push again.

---

## Task 7: Final Verification Checklist

Run through these manually:

- [ ] Homepage loads and shows recent posts
- [ ] Posts listing page works (`/posts/`)
- [ ] Hello World post renders with code highlighting
- [ ] Mermaid diagram renders correctly
- [ ] About page is accessible
- [ ] Dark/light mode toggle works
- [ ] Search works
- [ ] RSS feed available (`/index.xml`)
- [ ] Site is fast (check with PageSpeed Insights)

---

## Post-Setup: Writing New Posts

To create a new post:

```bash
hugo new posts/my-new-post/index.md
```

This creates `content/posts/my-new-post/index.md` with front matter. Write your content, add images to the same folder, then:

```bash
git add -A
git commit -m "post: title of your post"
git push
```

GitHub Actions handles the rest.

---

## Sources
- [Hugo: Host on GitHub Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/)
- [Blowfish Installation Guide](https://blowfish.page/docs/installation/)
- [Blowfish Configuration](https://blowfish.page/docs/configuration/)
- [Blowfish Diagrams & Flowcharts](https://blowfish.page/samples/diagrams-flowcharts/)
- [Hugo Installation on Windows](https://gohugo.io/installation/windows/)
