# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

A static marketing website for **Trinity Oaks Construction Company, LLC** (TOCC) — a locally-owned general contractor in Winston-Salem, NC. The site lives in `site/` and is plain HTML/CSS/JS with no build step.

The folder is named `handyman/` for historical reasons (started as handyman-services research, pivoted to a construction-company site). The `research/` folder retains the prior handyman pricing notes — keep them, they may be reused.

## Run / preview

There is no build, install, or test command. Serve the `site/` folder over HTTP — opening files via `file://` will silently break the SVG `background-image` URLs referenced by relative paths.

```powershell
cd site
python -m http.server 8080
# OR
npx --yes http-server -p 8080 -c-1
```

## Architecture

Six hand-written HTML pages, each fully self-contained:

- `index.html` — home (hero, service cards, why-us, about teaser, testimonials, CTA)
- `services.html` — full service list with anchored sections (`#custom-homes`, `#kitchen`, etc. — footer links rely on these)
- `rates.html` — public rate table for handyman/small-job services plus travel fees; sourced from `research/2026-05-14-155221-…md`
- `projects.html` — portfolio gallery (SVG placeholders; replace with real photos)
- `about.html` — story, four-step process, values
- `contact.html` — contact info + estimate form
- `404.html` — fallback

**No template engine.** The header (with nav) and footer are duplicated across every page. When editing nav links, footer columns, the phone number, the email, or the logo, **change all six pages plus `404.html`**. Use the search/replace tool.

**One stylesheet** at `css/styles.css`. Design tokens (colors, spacing, radius) are CSS custom properties on `:root`. Change a token once, the whole site updates. Color palette is "oak/sand/leaf" earth tones — keep that vocabulary consistent if extending.

**One JS file** at `js/nav.js`. Two responsibilities only:
1. Toggle the mobile nav drawer (`<860px` breakpoint).
2. Handle the contact form `submit` event by building a `mailto:` URL — no server.

**Responsive breakpoints:** 860px (collapses nav to hamburger, stacks two-column grids) and 500px (single-column footer, stacks hero buttons).

## Placeholders that must be replaced before publishing

These are intentionally fake — search and replace site-wide:

- `336-978-4041` — primary phone number (tel: link uses `+13369784041`)
- `info@trinityoaksconstruction.com` — placeholder email (also hardcoded in `js/nav.js` mailto fallback)
- All `site/images/project-*.svg` — SVG illustrations standing in for real photos
- `site/images/hero.svg`, `site/images/about.svg` — also placeholders
- Three testimonials on `index.html` — example copy

The `sitemap.xml` and `robots.txt` reference `https://trinityoaksconstruction.com/` — change if a different primary domain is registered (see `README.md` for domain status).

## Conventions to keep

- **No frameworks, no build step, no npm.** This site must remain editable by anyone who knows basic HTML. Don't introduce React, Tailwind, a static-site generator, or a bundler.
- **No CDN font/JS dependencies.** System fonts (Georgia/Times) only. No Google Fonts, no jQuery, no analytics scripts unless the owner explicitly asks.
- **Inline SVG icons** for service icons (already in the HTML). Don't pull in Font Awesome or an icon CDN.
- **No backend.** The contact form uses `mailto:`. If a real submission endpoint is needed, see the README (Formspree / Netlify Forms / Cloudflare Workers — pick one, don't invent custom infra).
- **Match existing card/section patterns** when adding content. The grid classes (`.services-grid`, `.features-grid`, `.gallery-grid`) auto-fit, so just add another card.

## Adding a new page

1. Copy `about.html` as a starting point (it has page-header + content + CTA + footer).
2. Update `<title>`, `<meta name="description">`, the `<h1>`, and the breadcrumb.
3. Add the page to the nav `<ul>` in **all six existing pages** plus `404.html`.
4. Add a `<url>` entry to `sitemap.xml`.

## Verifying changes

Before declaring work done, serve locally and check at minimum:

1. The page renders at desktop width (~1280px) and mobile width (~390px).
2. No console errors (`js/nav.js` should be silent).
3. All footer links and nav links resolve to a real page.
4. Anchor links from `services.html` (e.g. `#kitchen`, `#decks`) still scroll to the right section.
