# Goog Demo Project

This demonstrates how to use the **goog GitHub Action** (v2) to automatically generate Open Graph images from Markdown frontmatter.

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

The workflow at `.github/workflows/og-images.yml` uses the **riceball-tw/goog@v2** action:

```yaml
- id: generate
  uses: riceball-tw/goog@v2
  with:
    scan_markdown: content/blog    # Directory to scan for .md files
    template: templates/og.html    # Optional custom template
    post_preview: "true"           # Post image previews on PRs
- uses: actions/upload-artifact@v4
  with:
    name: og-images
    path: ${{ steps.generate.outputs.artifact_path }}
    if-no-files-found: error
```

> **PR previews** require `pull-requests: write` permission on the job. Add to your workflow:
> ```yaml
> jobs:
>   generate:
>     permissions:
>       contents: read
>       pull-requests: write
> ```

### Trigger Events
- **Push to main**: When markdown files or templates change
- **Pull requests**: On PRs touching markdown/templates

### What It Does
1. Scans `content/blog/` for `.md` files with `ogImage` frontmatter
2. Generates OG images using the specified template
3. Saves images to the artifact staging directory
4. Uploads generated images as a workflow artifact (downloadable from the run page)
5. Posts image previews as PR comments

## Required Frontmatter Format

Each markdown file needs an `ogImage` section. All keys under `ogImage` become template variables (use `{{.key_name}}` to render):

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
goog --scan-markdown content/blog --template templates/og.html

# Batch mode from JSON
goog --config images.json --workers 4
```

## Publishing the Action

To publish your own version to GitHub Marketplace:

1. **Create a new repo** (e.g., `my-org/goog-action`)
2. **Add action.yml** (already exists in parent repo root)
3. **Tag a release**:
    ```bash
    git tag v2.0.0
    git push origin v2.0.0
    ```
4. **Publish to Marketplace** (optional):
   - Go to repo Settings → GitHub Actions → Publish to Marketplace
   - Or users can reference directly: `uses: my-org/goog-action@v2`

## Action Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `scan_markdown` | No | - | Directory to scan for .md files |
| `config` | No | - | Path to JSON config file for batch generation |
| `template` | No | `templates/og.html` | Custom HTML template path |
| `workers` | No | `4` | Number of concurrent workers |
| `post_preview` | No | `"true"` | Post image previews on PRs |
| `artifact_path` | No | `goog-artifacts` | Directory for staging generated images |
| `ignore_patterns` | No | `README.md` | Comma-separated glob patterns to ignore |
| `github_token` | No | `${{ github.token }}` | GitHub token for PR comments |

## Action Outputs

| Output | Description |
|--------|-------------|
| `artifact_path` | Directory containing generated images (for `actions/upload-artifact`) |
| `generated_images` | Newline-separated list of generated image paths |