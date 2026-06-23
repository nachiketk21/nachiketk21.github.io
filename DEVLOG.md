# DEVLOG — nachiketk21.github.io

Technical reference for the personal portfolio/resume site at **nachiketkshirsagar.com**.  
Use this to recall what was built, why decisions were made, and how to work on it again.

---

## Overview

Jekyll-based static site hosted on **GitHub Pages**. Deployed automatically on every push to `master`. No Gemfile — the site relies entirely on GitHub Pages' default Jekyll environment (no local gem management needed for production; only needed if running locally).

Custom theme files live directly in the repo (not a gem-based theme). The original base is "Jekyll Mono", heavily modified.

---

## Tech Stack

| Layer | Choice |
|---|---|
| Generator | Jekyll (static site) |
| Hosting | GitHub Pages, custom domain via `CNAME` → `nachiketkshirsagar.com` |
| CSS | Sass/SCSS, compiled by Jekyll's built-in pipeline |
| Icons | Font Awesome 6.5.1 (CDN — `cdnjs.cloudflare.com`) |
| Font (resume) | Inter via Google Fonts (weights 300–700) |
| Font (blog) | Helvetica / Helvetica Neue (system font stack) |
| Plugins | `jekyll-sitemap`, `jekyll-feed` |

---

## File Map

```
nachiketk21.github.io/
├── _config.yml              ← site metadata, social links, plugin list
├── index.html               ← single-page resume (layout: home)
├── about.md                 ← meta-redirect to / (layout: null)
├── style.scss               ← root stylesheet; all @imports live here
│
├── _layouts/
│   ├── home.html            ← standalone layout for resume page (NEW June 2026)
│   ├── default.html         ← base layout for blog/standard pages
│   ├── page.html            ← simple static page layout
│   ├── blog.html            ← blog post listing layout
│   └── post.html            ← individual blog post layout
│
├── _sass/
│   ├── _resume.scss         ← all resume-specific styles (NEW June 2026)
│   ├── _variables.scss      ← color palette, font stacks, breakpoints, resume tokens
│   ├── _jekyll-mono.scss    ← original theme base styles
│   ├── _highlights.scss     ← syntax highlighting for code blocks
│   └── _reset.scss          ← CSS reset/normalization
│
├── _includes/
│   ├── svg-icons.html       ← footer social icons (driven by _config.yml footer-links)
│   ├── meta.html            ← Open Graph / meta tags partial
│   ├── analytics.html       ← Google Analytics (empty/disabled)
│   └── disqus.html          ← Disqus comments (empty/disabled)
│
├── _posts/                  ← two blog posts from 2018 (untouched)
├── assets/
│   └── Nachiket_Kshirsagar_Resume.pdf   ← downloadable PDF
├── images/                  ← avatar, background, post images
├── CNAME                    ← nachiketkshirsagar.com
└── CLAUDE.md                ← instructions for Claude Code AI assistant
```

---

## Homepage Redesign (June 2026)

**Before:** `index.html` was a meta-redirect to `/about/`, which had a profile card flip effect using the `default.html` layout.

**After:** `index.html` is the entire single-page scrolling resume. `about.md` now redirects back to `/`.

### Why a separate `home.html` layout?

`default.html` renders the Jekyll Mono blog header (site name + nav links). The resume needs its own fixed nav. Rather than conditionally hiding the old header, a new standalone `home.html` was created — it doesn't extend anything, so there's zero interference with existing blog post pages.

### Page Sections

| Section | Anchor | Notes |
|---|---|---|
| Hero | `#top` | `min-height: 100vh`, two CTA buttons |
| About | `#about` | Numbered 01 |
| Experience | `#experience` | Numbered 02, timeline component |
| Skills & Education | `#skills` | Numbered 03, CSS grid chip layout |
| Contact | `#contact` | Numbered 04 |

---

## CSS Architecture

### Isolation strategy

All resume styles use `r-` or `resume-` class prefixes. This avoids collisions with the existing blog styles that target bare element names (`h2`, `nav`, `a`, etc.) in `style.scss` and `_jekyll-mono.scss`.

BEM naming: `.resume-nav__inner`, `.r-hero__name`, `.r-timeline__item__dot`, etc.

`_resume.scss` is imported **last** in `style.scss` so it wins any cascade conflicts without needing `!important`.

### Design tokens

Added to the bottom of `_sass/_variables.scss` under a labeled section:

```scss
$r-font:           'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif;
$r-text-primary:   #1a202c;
$r-text-secondary: #4a5568;
$r-text-muted:     #718096;
$r-bg-alt:         #f8f9fc;   // alternating section background
$r-border:         #e2e8f0;
$r-accent:         #8476ad;   // purple — matches existing $mono-color blog theme
$r-accent-light:   #ede9f5;
$r-section-num:    #d4c5f0;   // decorative oversized section numbers
```

### Key CSS techniques used

- **`clamp(40px, 7vw, 78px)`** for the hero name — fluid responsive sizing without media queries or JS.
- **`backdrop-filter: blur(10px)`** on the fixed nav for the frosted glass effect.
- **`position: relative` + `::before` pseudo-element** on `.r-timeline` to draw the vertical connector line (2px wide, `left: 11px`, spanning `top: 12px` to `bottom: 12px`).
- **CSS Grid** (`auto-fill minmax(260px, 1fr)`) for the skills chip groups.
- **`margin-left: auto`** on `.r-timeline__date` to push the date to the right on desktop; reset to 0 on mobile.
- Single responsive breakpoint at `640px` via the existing `@include mobile {}` mixin in `_variables.scss`.

### Import order in style.scss

```scss
@import "reset";
@import "variables";
// ... base rules inline ...
@import "highlights";
@import "jekyll-mono";
@import "resume";    ← last, so resume rules override everything above
```

---

## Timeline Component

`.r-timeline` structure:

```html
<div class="r-timeline">
  <div class="r-timeline__item">
    <div class="r-timeline__dot"></div>       <!-- circle bullet -->
    <div class="r-timeline__body">
      <div class="r-timeline__meta">
        <h3 class="r-timeline__role">...</h3>
        <span class="r-timeline__company">...</span>
        <span class="r-timeline__date">...</span>
      </div>
      <ul class="r-timeline__bullets">...</ul>
    </div>
  </div>
</div>
```

The vertical line is a `::before` pseudo on `.r-timeline`, not an SVG or border — this means it reuses the same `$r-border` color and auto-sizes to the content height.

---

## Social Icons

`_includes/svg-icons.html` outputs Font Awesome icons for every non-empty key under `footer-links` in `_config.yml`. To add or remove a social link, edit `_config.yml` only — no template changes needed.

Current configured links: email, GitHub, Instagram, LinkedIn, Pinterest, Twitter (X), Stack Overflow.

---

## Known Warnings (non-breaking)

Dart Sass emits deprecation warnings on every build:

- **`darken()` deprecated** — used in `_jekyll-mono.scss` (pre-existing) and in `_resume.scss` for button/link hover states.
- **`@import` deprecated** — used in `style.scss` (pre-existing in the theme).

These are warnings, not errors. Build succeeds. Fix path: migrate `darken($r-accent, 8%)` to `color.adjust($r-accent, $lightness: -8%)` and `@import` to `@use`/`@forward` when doing a future Sass cleanup.

---

## Local Development

```bash
# One-time setup
gem install jekyll jekyll-sitemap jekyll-feed

# Serve (jekyll may not be in PATH on macOS — use full path)
/opt/homebrew/lib/ruby/gems/4.0.0/bin/jekyll serve

# → http://localhost:4000
```

- Most file changes hot-reload automatically.
- `_config.yml` changes require a full server restart.
- `_site/` is gitignored — the build output is never committed.

---

## Deployment

GitHub Pages auto-deploys ~1-2 min after every push to `master`.

The remote must be pushed via the personal SSH alias (not `github.com` directly, which routes to the Domo work account):

```bash
git push git@github-personal:nachiketk21/nachiketk21.github.io.git master
```

The `github-personal` host alias is defined in `~/.ssh/config`:

```
Host github-personal
  HostName github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519_personal_github
  IdentitiesOnly yes
```

---

## Key Commits

| Commit | What |
|---|---|
| `dda4173` | Resume content update from PDF — expanded bullets, corrected dates, full education |
| `30bd414` | **Major redesign** — new `home.html` layout, `_resume.scss`, `index.html` as single-page resume |
| `c182a5a` | Polish about.md professional copy |
| `3a5ce19` | Remove projects page, stop tracking `_site/` |
| `cea3039` | Modernize site: fix broken links, update styling, add `CLAUDE.md` |
