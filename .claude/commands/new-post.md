---
name: new-post
description: Use when creating a new blog post for the Offensive Programming Blog. Handles post scaffolding, frontmatter, content structure, and publishing.
---

# New Blog Post

Create a new post for the Hugo + Blowfish blog at `C:\Users\Mistress\claude_projects\blog`.

## Arguments

The user may provide: a topic, a title, or just say "new post". If no topic is given, ask for one.

## Steps

### 1. Gather Info (ask only what's missing)

- **Title** (required)
- **Topic/outline** (required — what the post is about)
- **Tags** (optional — suggest based on topic)

### 2. Create Post Directory

```bash
export PATH="/c/Users/Mistress/AppData/Local/Microsoft/WinGet/Packages/Hugo.Hugo.Extended_Microsoft.Winget.Source_8wekyb3d8bbwe:$PATH"
hugo new posts/<slug>/index.md
```

The slug should be lowercase, hyphenated (e.g., `debugging-race-conditions`).

### 3. Write Frontmatter

Replace the generated frontmatter with:

```yaml
---
title: "Post Title Here"
date: YYYY-MM-DD
draft: false
description: "One-sentence summary for SEO and previews."
tags: ["tag1", "tag2"]
---
```

Set `draft: false` unless the user says it's a draft.

### 4. Write Content

- Use standard markdown with `##` for sections
- For code blocks: use fenced blocks with language identifier
- For mermaid diagrams: use the Blowfish shortcode:

```
{{</* mermaid */>}}
graph LR
    A --> B
{{</* /mermaid */>}}
```

- For images: place files in the same directory as `index.md` and reference as `![alt](filename.jpg)`

### 5. Verify Build

```bash
export PATH="/c/Users/Mistress/AppData/Local/Microsoft/WinGet/Packages/Hugo.Hugo.Extended_Microsoft.Winget.Source_8wekyb3d8bbwe:$PATH"
hugo --gc --minify
```

### 6. Ask About Publishing

Ask the user: "Post is ready. Should I commit and push to publish it live?"

If yes:
```bash
git add content/posts/<slug>/
git commit -m "post: <title>"
git push
```

## Reference: Blowfish Frontmatter Options

| Key | Type | Notes |
|-----|------|-------|
| `title` | string | Required |
| `date` | YYYY-MM-DD | Required |
| `draft` | bool | Set false to publish |
| `description` | string | Used for SEO meta + previews |
| `tags` | list | e.g. `["go", "debugging"]` |
| `categories` | list | Broader grouping |
| `series` | list | Multi-part post series |
| `showTableOfContents` | bool | Override site default |
| `showReadingTime` | bool | Override site default |
| `showDate` | bool | Override site default |

## Reference: Shortcodes

| Shortcode | Usage |
|-----------|-------|
| Mermaid | `{{</* mermaid */>}} ... {{</* /mermaid */>}}` |
| Alert | `{{</* alert */>}} text {{</* /alert */>}}` |
| Badge | `{{</* badge */>}} text {{</* /badge */>}}` |
| Button | `{{</* button href="url" */>}} text {{</* /button */>}}` |
| Figure | `{{</* figure src="image.jpg" alt="desc" */>}}` |
| KaTeX | `{{</* katex */>}} E=mc^2 {{</* /katex */>}}` |
| Lead | `{{</* lead */>}} intro text {{</* /lead */>}}` |
