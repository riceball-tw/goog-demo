---
title: "Advanced Goog Usage"
ogImage:
  title: "Advanced Goog Usage Demo"
  tag: "Advanced"
  description: "Batch generation, custom templates, and CI/CD integration"
  site_name: "myblog.dev"
  template: "templates/og.html"
---

# Advanced Goog Usage

Take your OG image generation to the next level...

## Batch Generation

Create an `images.json` file:

```json
[
  {
    "vars": {
      "title": "Post 1",
      "description": "First post",
      "tag": "Blog",
      "site_name": "myblog.dev"
    },
    "out": "out/post1.png"
  },
  {
    "vars": {
      "title": "Post 2",
      "description": "Second post",
      "tag": "Blog",
      "site_name": "myblog.dev"
    },
    "out": "out/post2.png"
  }
]
```

Run with multiple workers:

```bash
goog --config images.json --workers 8
```

## Custom Templates

Create your own HTML template using Go's `text/template` syntax...