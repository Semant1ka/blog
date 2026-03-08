# Design: Offensive Programming Blog

**Date:** 2026-03-07
**Status:** Approved

## Overview
Personal blog for a Staff Developer. Mixed content — mostly text with code snippets and mermaid diagrams. Forever free, fully owned, discoverable.

## Architecture
- **Static site generator:** Hugo
- **Theme:** Blowfish (Tailwind CSS, mermaid/KaTeX built-in)
- **Hosting:** GitHub Pages (username.github.io)
- **Deployment:** GitHub Actions — push to main, auto-builds and deploys
- **Content format:** Markdown files in git repo

## Site Structure
```
/                    -> Homepage (recent posts + intro)
/posts/              -> Blog listing (all posts, paginated)
/posts/my-article/   -> Individual post (markdown + images + mermaid)
/about/              -> About me page
```

## Content Workflow
1. Write markdown in content/posts/my-post/index.md
2. Drop images in the same folder (Hugo page bundles)
3. Use mermaid code blocks for diagrams (rendered automatically by Blowfish)
4. git push -> GitHub Actions builds -> live on GitHub Pages

## Features (from Blowfish, zero config)
- Light/dark mode (system preference)
- Mermaid diagrams, code syntax highlighting
- Table of contents on posts
- Search
- SEO (sitemap, OpenGraph, Twitter cards)
- RSS feed

## Discoverability
- SEO handled by Hugo + Blowfish out of the box
- Cross-post to dev.to / Hashnode with canonical URL pointing back to blog

## Repo Structure
```
blog/
├── .github/workflows/   -> GitHub Actions deploy workflow
├── content/
│   ├── posts/           -> Blog posts (markdown)
│   └── about.md         -> About page
├── static/              -> Global static assets
├── hugo.toml            -> Hugo + Blowfish config
└── themes/blowfish/     -> Theme (git submodule)
```

## Decisions
- Hugo over Jekyll/Astro: fastest builds, no runtime deps, Go-based (user prefers Go over JS)
- Blowfish over PaperMod: built-in mermaid, Tailwind, more features out of the box
- GitHub Pages over Netlify/Vercel: simplest free hosting, no vendor lock-in
- Git-based writing over Notion CMS: full ownership, no API dependency
