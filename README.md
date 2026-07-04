# Goog Demo Project

This demonstrates how to use the **goog GitHub Action** to automatically generate Open Graph images from Markdown frontmatter.

## Project Structure

```
demo/
├── content/
│   └── blog/
│       ├── getting-started.md
│       ├── advanced-usage.md
│       └── ci-cd-integration.md
├── templates/
│   └── og.html          # Custom OG image template
├── images.json          # Batch generation config
├── .github/
│   └── workflows/
│       └── og-images.yml # GitHub Action workflow
└── README.md
```

## How the GitHub Action Works

The workflow at `.github/workflows/og-images.yml` uses the **riceball-tw/goog@v1** action:

```yaml
- uses: riceball-tw/goog@v1
  with:
    scan_markdown: content/blog    # Directory to scan for .md files
    template: templates/og.html    # Optional custom template
    commit: "true"                 # Auto-commit generated images
    commit_message: "chore: update OG images [skip ci]"
    github_token: ${{ secrets.GITHUB_TOKEN }}
    post_preview: "true"           # Post image previews on PRs
```

### Trigger Events
- **Push to main**: When markdown files or templates change
- **Pull requests**: On PRs touching markdown/templates

### What It Does
1. Scans `content/blog/` for `.md` files with `ogImage` frontmatter
2. Generates OG images using the specified template
3. Saves images to `public/og/<slug>.png` (or next to source)
4. Commits changes back to the repo (with `[skip ci]`)
5. Posts image previews as PR comments

## Required Frontmatter Format

Each markdown file needs an `ogImage` section:

```yaml
---
title: "Your Post Title"
ogImage:
  title: "OG Image Title"
  tag: "Category"
  description: "Description for social cards"
  site_name: "yourdomain.com"
  template: "templates/og.html"  # optional, per-file override
---
```

## Running Locally

```bash
# Install goog
go install github.com/riceball-tw/goog/cmd/goog@latest

# Generate from markdown frontmatter
goog --scan_markdown content/blog --template templates/og.html

# Batch mode from JSON
goog --config images.json --workers 4
```

## Publishing the Action

To publish your own version to GitHub Marketplace:

1. **Create a new repo** (e.g., `my-org/goog-action`)
2. **Add action.yml** (already exists in parent repo root)
3. **Tag a release**:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```
4. **Publish to Marketplace** (optional):
   - Go to repo Settings → GitHub Actions → Publish to Marketplace
   - Or users can reference directly: `uses: my-org/goog-action@v1`

## Action Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `scan_markdown` | Yes | - | Directory to scan for .md files |
| `template` | No | default template | Custom HTML template path |
| `commit` | No | `"false"` | Auto-commit generated images |
| `commit_message` | No | `"chore: update OG images"` | Commit message |
| `github_token` | If commit=true | - | Token for pushing commits |
| `post_preview` | No | `"false"` | Post image previews on PRs |
| `workers` | No | `4` | Parallel workers for generation |