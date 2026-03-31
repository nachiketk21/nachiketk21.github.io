# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Jekyll-based personal portfolio/resume website hosted on GitHub Pages at `nachiketkshirsagar.com`. It uses a custom "Jekyll Mono" theme (not a gem-based theme — all theme files live directly in the repo).

## Local Development

No Gemfile is present; the site relies on GitHub Pages' default Jekyll environment. To run locally:

```bash
gem install jekyll jekyll-sitemap jekyll-feed
jekyll serve
```

The site is then available at `http://localhost:4000`. Changes to most files are picked up automatically without restarting. Deployment happens automatically on every push to `master` via GitHub Pages.

## Architecture

- `_config.yml` — Site metadata (name, description, social links, plugins). The `url` field is intentionally left blank (GitHub Pages sets it).
- `_layouts/` — Four layouts: `default.html` (wraps everything), `page.html` (static pages), `blog.html` (post listing), `post.html` (individual posts).
- `_includes/` — Reusable partials (header, footer, disqus, analytics).
- `_sass/` — SCSS partials imported into `style.scss`. Primary theme color is `#8476ad`; alternatives are commented out in the sass files.
- `_posts/` — Blog posts in `YYYY-MM-DD-title.md` format.
- `_site/` — Build output tracked in git (not gitignored).

## Content Files

The primary content is in Markdown pages at the repo root:
- `about.md` — Main professional profile and resume content (most frequently edited).
- `projects.md` — Projects page.
- `index.html` — Homepage with the profile card flip effect.

## Key Configuration Notes

- Disqus and Google Analytics are configured in `_config.yml` but left empty (disabled).
- Permalinks use `/:title/` format.
- Sass output is `:expanded` (not minified) — change to `:compressed` for production optimization if needed.
- `jekyll-sitemap` and `jekyll-feed` are the only active plugins.
