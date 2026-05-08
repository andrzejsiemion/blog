# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal blog/portfolio site at siemion.pro built with **Hugo** (v0.154.2 extended) using the **PaperMod** theme (included as a git submodule). Deployed automatically to GitHub Pages on push to `main`.

## Commands

```bash
# Start local development server
hugo server

# Build site for production
hugo --gc --minify --baseURL "https://siemion.pro/"

# Create new content from archetype
hugo new content/blog/YYYY-NNN.md
```

Hugo extended v0.154.2 is required (extended for SCSS support).

## Content Structure

- `content/blog/` — Blog posts (naming convention: `YYYY-NNN.md`, e.g. `2026-001.md`)
- `content/projects/` — Projects section with custom layout
- `content/about.md` — About page

New posts are created as `draft: true` by default. Set `draft: false` to publish.

## Architecture

**Theme**: PaperMod runs in profile mode — the homepage shows a profile card instead of a post list. Theme lives in `themes/PaperMod/` as a git submodule; do not edit files there.

**Customizations live in**:
- `layouts/shortcodes/` — Custom shortcodes (e.g. `social-icons.html` renders GitHub/LinkedIn icons)
- `hugo.yaml` — All site configuration including menus, social links, and PaperMod params

**Build output**: `public/` directory — committed to the repo as GitHub Pages deployment artifact. The CI workflow uploads this directory as a GitHub Pages artifact.

## Deployment

Pushing to `main` triggers `.github/workflows/main.yml` which builds with Hugo and deploys to GitHub Pages. The `public/` directory is also tracked in git (contains the pre-built site).

## Theme Submodule

When cloning or after pulling changes that update the submodule:
```bash
git submodule update --init --recursive
```
