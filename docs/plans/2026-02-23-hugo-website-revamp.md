# Hugo Website Revamp Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Replace the existing Astro-based personal website with a Hugo + PaperMod static site featuring blog, projects, bookshelf, and contact sections.

**Architecture:** Hugo static site generator with PaperMod theme added as a git submodule. Content is authored in Markdown under `content/`. GitHub Actions builds and deploys to GitHub Pages at `www.stark-dev.com`.

**Tech Stack:** Hugo, PaperMod theme, GitHub Actions, GitHub Pages

---

### Task 1: Remove old Astro project files

**Files:**
- Delete: `src/` (entire directory)
- Delete: `astro.config.ts`
- Delete: `tailwind.config.cjs`
- Delete: `tsconfig.json`
- Delete: `package.json`
- Delete: `package-lock.json` (if exists)
- Delete: `node_modules/` (if exists)
- Delete: `Dockerfile`
- Delete: `docker-compose.yml`
- Delete: `.github/workflows/ci.yml`
- Delete: `.github/workflows/deploy.yml`
- Delete: `public/` (entire directory)
- Keep: `CNAME`, `.git/`, `README.md`, `.gitignore`, `docs/`

**Step 1: Delete all Astro-related files and directories**

```bash
cd /Users/himanshu/dev/documents/himanshuMaheshwari2311.github.io
rm -rf src/ public/ node_modules/
rm -f astro.config.ts tailwind.config.cjs tsconfig.json package.json package-lock.json Dockerfile docker-compose.yml
rm -f .github/workflows/ci.yml .github/workflows/deploy.yml
```

**Step 2: Commit the cleanup**

```bash
git add -A
git commit -m "chore: remove old Astro project files"
```

---

### Task 2: Initialize Hugo project and add PaperMod theme

**Files:**
- Create: `hugo.yml`
- Create: `archetypes/default.md`
- Create: `themes/PaperMod/` (git submodule)
- Create: `content/` directory structure
- Create: `assets/css/extended/` directory
- Create: `layouts/` directory
- Create: `static/` directory

**Step 1: Install Hugo (if not already installed)**

```bash
brew install hugo
hugo version
```

Expected: `hugo v0.1xx.x+extended ...`

**Step 2: Initialize Hugo project in the existing repo**

Hugo's `hugo new site` won't work in a non-empty directory, so we create the structure manually.

```bash
cd /Users/himanshu/dev/documents/himanshuMaheshwari2311.github.io
mkdir -p archetypes content/posts content/bookshelf assets/css/extended layouts static themes
```

**Step 3: Create the default archetype**

Create `archetypes/default.md`:

```markdown
---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
draft: true
description: ""
tags: []
---
```

**Step 4: Add PaperMod as a git submodule**

```bash
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

**Step 5: Verify PaperMod was added**

```bash
ls themes/PaperMod/
```

Expected: Should see `layouts/`, `assets/`, `theme.toml`, etc.

**Step 6: Commit**

```bash
git add -A
git commit -m "feat: initialize Hugo project with PaperMod theme"
```

---

### Task 3: Create Hugo site configuration

**Files:**
- Create: `hugo.yml`

**Step 1: Create `hugo.yml` with full site configuration**

```yaml
baseURL: "https://www.stark-dev.com/"
title: "Himanshu Maheshwari"
copyright: "© 2026 Himanshu Maheshwari"
theme: ["PaperMod"]
languageCode: en-us

enableInlineShortcodes: true
enableRobotsTXT: true
enableEmoji: true
buildDrafts: false
buildFuture: false
buildExpired: false
pygmentsUseClasses: true

mainsections: ["posts"]

pagination:
  pagerSize: 10

outputs:
  home:
    - HTML
    - RSS
    - JSON

taxonomies:
  tag: tags
  category: categories

menu:
  main:
    - name: Blog
      url: /posts/
      weight: 1
    - name: Projects
      url: /projects/
      weight: 2
    - name: Bookshelf
      url: /bookshelf/
      weight: 3
    - name: About
      url: /about/
      weight: 4
    - name: Search
      url: /search/
      weight: 5

markup:
  goldmark:
    renderer:
      unsafe: true
  highlight:
    noClasses: false

minify:
  disableXML: true

params:
  env: production
  description: "Backend software engineer. Building scalable systems with Kotlin, Spring, AWS, and Kubernetes."
  author: "Himanshu Maheshwari"
  defaultTheme: auto
  disableThemeToggle: false

  ShowReadingTime: true
  ShowShareButtons: false
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowCodeCopyButtons: true
  ShowWordCount: false
  ShowRssButtonInSectionTermList: true
  ShowAllPagesInArchive: true
  ShowToc: true
  TocOpen: false

  homeInfoParams:
    Title: "Hi, I'm Himanshu"
    Content: >
      Backend software engineer based in Amsterdam.
      I build scalable systems with Kotlin, Spring, AWS, and Kubernetes.


      Welcome to my corner of the internet where I write about
      software engineering, books, and ideas.

  socialIcons:
    - name: github
      url: "https://github.com/himanshuMaheshwari2311"
    - name: linkedin
      url: "https://linkedin.com/in/maheshwari-himanshu"
    - name: email
      url: "mailto:stark.9190.nl@gmail.com"

  cover:
    hidden: false
    hiddenInList: false
    hiddenInSingle: false

  fuseOpts:
    isCaseSensitive: false
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 1
    limit: 10
    keys: ["title", "permalink", "summary", "content"]

  assets:
    disableHLJS: true
```

**Step 2: Verify the config is valid**

```bash
hugo config
```

Expected: No errors, prints resolved config.

**Step 3: Commit**

```bash
git add hugo.yml
git commit -m "feat: add Hugo site configuration with PaperMod params"
```

---

### Task 4: Create content pages (About, Projects, Search)

**Files:**
- Create: `content/about.md`
- Create: `content/projects.md`
- Create: `content/search.md`

**Step 1: Create the About page**

Create `content/about.md`:

```markdown
---
title: "About"
description: "A little about me"
hidemeta: true
ShowBreadCrumbs: false
---

Hi, I'm Himanshu Maheshwari, a software engineer based in Amsterdam with a passion for building scalable web applications and innovative tools.

I love building and experimenting with new ideas and solving complex problems.

## Tech Stack

I have experience working with:

- **Languages:** Kotlin, Java
- **Frameworks:** Spring, Quarkus, Vert.x
- **Cloud:** AWS
- **Data:** PostgreSQL, Kafka
- **Infrastructure:** Kubernetes, Docker
- **Architecture:** Microservices, Event-Driven Systems

... and a lot more tools and technologies in the microservices space.

I'm always eager to contribute to the open-source community and share what I learn along the way.
```

**Step 2: Create the Projects page**

Create `content/projects.md`:

```markdown
---
title: "Projects"
description: "Things I've built"
hidemeta: true
ShowBreadCrumbs: false
---

## Featured Projects

*Coming soon — curated projects with descriptions, tech stack, and links.*

---

Check out more of my work on [GitHub](https://github.com/himanshuMaheshwari2311).
```

**Step 3: Create the Search page**

Create `content/search.md`:

```markdown
---
title: "Search"
layout: "search"
summary: "search"
placeholder: "Search posts..."
---
```

**Step 4: Verify the site builds and pages render**

```bash
hugo server -D
```

Expected: Site runs at `http://localhost:1313/`. Navigate to `/about/`, `/projects/`, `/search/` — all should render.

**Step 5: Commit**

```bash
git add content/about.md content/projects.md content/search.md
git commit -m "feat: add About, Projects, and Search pages"
```

---

### Task 5: Create Bookshelf section

The bookshelf is a list section where each book review is a separate markdown file.

**Files:**
- Create: `content/bookshelf/_index.md`

**Step 1: Create the bookshelf section index**

Create `content/bookshelf/_index.md`:

```markdown
---
title: "Bookshelf"
description: "Books I've read with mini reviews and takeaways"
hidemeta: true
ShowBreadCrumbs: false
---

Books I've read and what I took away from them. Each review is a short summary with my personal takeaways.
```

**Step 2: Create a sample book review**

Create `content/bookshelf/designing-data-intensive-applications.md`:

```markdown
---
title: "Designing Data-Intensive Applications"
date: 2026-02-23
description: "Martin Kleppmann's guide to the principles behind reliable, scalable, and maintainable data systems"
tags: ["system-design", "distributed-systems", "books"]
ShowToc: false
---

**Author:** Martin Kleppmann

**Rating:** 5/5

## Summary

A deep dive into the principles and practicalities of data systems — from storage engines and data models to distributed consensus and stream processing.

## Key Takeaways

- *Add your takeaways here*

## Who Should Read This

Anyone building or maintaining backend systems that deal with data at scale.
```

**Step 3: Verify bookshelf renders**

```bash
hugo server -D
```

Expected: Navigate to `/bookshelf/` — should show the section page with the sample review listed.

**Step 4: Commit**

```bash
git add content/bookshelf/
git commit -m "feat: add Bookshelf section with sample review"
```

---

### Task 6: Create sample blog post

**Files:**
- Create: `content/posts/introduction.md`

**Step 1: Create the introductory blog post**

Migrated from the existing Astro blog post content.

Create `content/posts/introduction.md`:

```markdown
---
title: "Introduction"
date: 2024-09-23
description: "Welcome to my blog — what this space is about and what you can expect"
tags: ["intro", "personal"]
draft: false
ShowToc: false
---

Welcome to my blog! This is my corner of the internet where I share my thoughts, experiences, and learnings as a software engineer.

## What to Expect

Here, you'll find:

- **Technical deep dives** — exploring backend technologies, distributed systems, and cloud architecture
- **Book summaries** — key takeaways from books I've read
- **Learnings** — things I've picked up while experimenting with new tools and technologies

I believe in learning by doing and sharing knowledge along the way. Whether you're a fellow engineer or just curious about tech, I hope you find something useful here.

Stay tuned for more posts!
```

**Step 2: Verify the post renders**

```bash
hugo server
```

Expected: Home page shows the post in "Recent Posts". Navigate to `/posts/introduction/` — post renders with reading time and tags.

**Step 3: Commit**

```bash
git add content/posts/introduction.md
git commit -m "feat: add introductory blog post"
```

---

### Task 7: Set up static assets and CNAME

**Files:**
- Move: `CNAME` to `static/CNAME`
- Create: `.gitignore` (update for Hugo)

**Step 1: Move CNAME to static directory**

Hugo copies everything from `static/` to the build output root. The CNAME file must be at the root for GitHub Pages custom domains.

```bash
mv CNAME static/CNAME
```

**Step 2: Update `.gitignore` for Hugo**

Create/replace `.gitignore`:

```
# Hugo build output
public/
resources/_gen/

# OS files
.DS_Store
Thumbs.db

# IDE
.idea/
.vscode/

# Hugo lock
.hugo_build.lock
```

**Step 3: Verify CNAME ends up in build output**

```bash
hugo --minify
ls public/CNAME
```

Expected: `public/CNAME` exists and contains `www.stark-dev.com`

**Step 4: Commit**

```bash
git add static/CNAME .gitignore
git commit -m "feat: move CNAME to static and update .gitignore"
```

---

### Task 8: Create GitHub Actions deployment workflow

**Files:**
- Create: `.github/workflows/deploy.yml`

**Step 1: Create the Hugo deploy workflow**

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy Hugo to GitHub Pages

on:
  push:
    branches:
      - master
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
      HUGO_VERSION: 0.147.0
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Install Hugo
        run: |
          curl -sLJO "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          mkdir -p "${HOME}/.local/hugo"
          tar -C "${HOME}/.local/hugo" -xf "hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          rm "hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          echo "${HOME}/.local/hugo" >> "${GITHUB_PATH}"

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build
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

**Step 2: Verify workflow syntax**

```bash
cat .github/workflows/deploy.yml
```

Expected: Valid YAML, no syntax errors.

**Step 3: Commit**

```bash
git add .github/workflows/deploy.yml
git commit -m "feat: add GitHub Actions workflow for Hugo deployment"
```

---

### Task 9: Local build verification

**Step 1: Clean and build the full site**

```bash
cd /Users/himanshu/dev/documents/himanshuMaheshwari2311.github.io
hugo --gc --minify
```

Expected: Build succeeds with output like:
```
                   | EN
-------------------+------
  Pages            |  XX
  Paginator pages  |   0
  Non-page         |  XX
  Static files     |  XX
  Processed images |   0
  Aliases          |  XX
  Cleaned          |   0
```

**Step 2: Verify key output files exist**

```bash
ls public/index.html
ls public/posts/index.html
ls public/about/index.html
ls public/projects/index.html
ls public/bookshelf/index.html
ls public/search/index.html
ls public/index.json
ls public/CNAME
ls public/sitemap.xml
ls public/robots.txt
```

Expected: All files exist.

**Step 3: Run local server and visually verify**

```bash
hugo server
```

Expected: Site at `http://localhost:1313/` with:
- Home page: greeting + social icons + recent post
- Navigation: Blog, Projects, Bookshelf, About, Search
- Dark/light toggle in header
- `/posts/introduction/` renders with tags and reading time
- `/about/` shows bio and tech stack
- `/projects/` shows placeholder
- `/bookshelf/` lists sample review
- `/search/` has working search box

**Step 4: Final commit (if any adjustments needed)**

```bash
git add -A
git commit -m "chore: final adjustments after local verification"
```

---

### Task 10: Clean up and update README

**Files:**
- Modify: `README.md`

**Step 1: Update README with new project info**

Replace `README.md` contents with:

```markdown
# stark-dev.com

Personal website and blog built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Local Development

```bash
# Install Hugo
brew install hugo

# Clone with submodules
git clone --recurse-submodules <repo-url>

# Run dev server
hugo server -D

# Build for production
hugo --gc --minify
```

## Adding Content

### Blog post
```bash
hugo new content/posts/my-new-post.md
```

### Book review
```bash
hugo new content/bookshelf/book-title.md
```

## Deployment

Pushes to `master` trigger automatic deployment to GitHub Pages via GitHub Actions.
```

**Step 2: Commit**

```bash
git add README.md
git commit -m "docs: update README for Hugo setup"
```
