---
title: "CI/CD Integration with GitHub Actions"
ogImage:
  title: "CI/CD Integration"
  tag: "DevOps"
  description: "Automate OG image generation in your CI/CD pipeline"
  site_name: "myblog.dev"
  template: "templates/og.html"
---

# CI/CD Integration with GitHub Actions

Automate OG image generation...

## GitHub Action Setup

Add a workflow file at `.github/workflows/og-images.yml`:

```yaml
name: Generate OG Images
on:
  push:
    branches: [main]
    paths:
      - 'content/**/*.md'
  pull_request:
    paths:
      - 'content/**/*.md'

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - id: generate
        uses: riceball-tw/goog@v2
        with:
          scan_markdown: content/blog
      - uses: actions/upload-artifact@v4
        with:
          name: og-images
          path: ${{ steps.generate.outputs.artifact_path }}
          if-no-files-found: error
```

## PR Previews

The action automatically posts image previews on pull requests...