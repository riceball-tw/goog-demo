---
title: "Getting Started with Goog"
ogImage:
  title: "Getting Started with Goog"
  tag: "Tutorial"
  description: "Learn how to generate beautiful Open Graph images automatically"
  site_name: "myblog.dev"
  template: "templates/og.html"
---

# Getting Started with Goog

Goog (Go OG) is a powerful tool for generating Open Graph social card images...

## Installation

```bash
go install github.com/riceball-tw/goog/cmd/goog@latest
```

## Quick Start

Generate your first OG image:

```bash
goog --title "Hello World" --desc "My first OG image" --out hello.png
```

## Next Steps

Check out the [configuration guide](/config-guide) for advanced usage.