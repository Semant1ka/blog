---
title: "Hello World"
date: 2026-03-07
description: "First post — testing markdown, code, and mermaid diagrams."
tags: ["meta"]
---

Welcome to Offensive Programming Blog. This is the first post — a quick test of the features this blog supports.

## Code Blocks

Here's some Go, because why not:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Offensive Programming Blog!")
}
```

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
