# siemion.pro

Source for [siemion.pro](https://siemion.pro) — my personal site and notepad. Built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, deployed to GitHub Pages on every push to `main`.

## Sections

- **Blog** — `content/blog/` (filenames follow `YYYY-NNN.md`, e.g. `2026-001.md`). Posts are created with `draft: true` and only publish once that flag is set to `false`.
- **Projects** — `content/projects/` with a custom layout for project pages.
- **About** — `content/about.md`.

## Local development

Requires Hugo Extended **v0.154.2** (extended needed for SCSS).

```bash
# Run a local server with live reload
hugo server

# Create a new blog post
hugo new content/blog/2026-002.md

# Production build
hugo --gc --minify --baseURL "https://siemion.pro/"
```

After cloning, initialise the PaperMod submodule:

```bash
git submodule update --init --recursive
```

## Support

If something here saved you time and you'd like to say thanks, you can [buy me a coffee](https://buymeacoffee.com/andrzejsiemion). The same link sits in the site footer.
