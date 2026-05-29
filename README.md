# Trinity Oaks Construction Company — Website

A static marketing site for Trinity Oaks Construction Company, LLC (Winston-Salem, NC).

## What's here

```
handyman/
├── site/                  ← the website (deploy this folder)
│   ├── index.html         ← homepage
│   ├── services.html      ← services overview
│   ├── projects.html      ← portfolio gallery
│   ├── about.html         ← about / process / values
│   ├── contact.html       ← contact + estimate form
│   ├── 404.html
│   ├── robots.txt
│   ├── sitemap.xml
│   ├── css/styles.css     ← all styles (single file)
│   ├── js/nav.js          ← mobile nav + contact-form mailto handler
│   └── images/            ← placeholder SVG illustrations
└── research/              ← prior research notes (handyman pricing, etc.)
```

No build step. Pure HTML/CSS/vanilla JS — drag-and-drop deployable.

## Run locally

Any static server works. Two zero-install options:

```powershell
# Python (already on most systems)
cd site
python -m http.server 8080
# → open http://localhost:8080

# OR with Node
cd site
npx --yes http-server -p 8080 -c-1
```

## Deploy

The `site/` folder deploys as-is to any static host. Recommended free options:

| Host | How |
| --- | --- |
| **Cloudflare Pages** | Create project, point at the repo, set build output dir to `site/`. Free, fast, includes free SSL. |
| **Netlify** | Drag-and-drop the `site/` folder at app.netlify.com, or connect git and set publish dir to `site`. |
| **GitHub Pages** | Push repo, settings → Pages → source = `main` branch, folder = `/site`. |
| **Vercel** | Import repo, set output dir to `site/`. |

## Domain name

Verified via the Verisign RDAP service on 2026-05-19:

| Domain | Status | Notes |
| --- | --- | --- |
| `tocc.com` | **Taken** | Registered since 1996, locked to prevent transfer — not realistically obtainable. |
| `trinityoaksconstruction.com` | **Available** | Recommended primary — full, descriptive, SEO-friendly. |
| `trinityoakscc.com` | **Available** | Shorter alternative using "CC" for Construction Company. |
| `trinityoaksbuild.com` | **Available** | Shorter, action-oriented. |
| `trinityoaksnc.com` | **Available** | Geo-anchored to NC. |
| `toccnc.com` | **Available** | Short acronym + state. |
| `tocc-nc.com` | **Available** | Acronym + state (hyphenated). |
| `buildtocc.com` | **Available** | Acronym variant. |

**Recommendation:** Register `trinityoaksconstruction.com` (primary) and `trinityoakscc.com` (short redirect). The site's metadata, sitemap, and contact email all reference `trinityoaksconstruction.com` — change them with find-and-replace if you choose a different primary.

Expect to pay ~$10–15/year per .com at any reputable registrar (Cloudflare Registrar sells at wholesale, Porkbun and Namecheap are also good). Avoid GoDaddy for renewals (price increases).

## Before going live

These placeholders MUST be updated with real values:

- **Phone:** `336-978-4041` — currently set site-wide (footer of every page + contact page). Update with find-and-replace if it changes.
- **Email:** `info@trinityoaksconstruction.com` — footer, contact page, `js/nav.js` mailto fallback.
- **Project photos:** `site/images/project-*.svg` are placeholder illustrations. Replace with real job-site photos (JPG/WebP). Update `<div class="img">` `background-image` URLs in `projects.html`.
- **Hero & about images:** `site/images/hero.svg`, `site/images/about.svg` — replace with photographs.
- **Testimonials:** the three on `index.html` are example copy. Replace with real customer quotes (with permission).
- **License #:** if NC requires display, add to `contact.html` "Licensed & Insured" block.
- **Service area:** verify the county list on `contact.html` matches where you actually take jobs.

## Editing pages

Each HTML page has the same shared structure: header (with nav), `<main>` with one or more `<section>` blocks, then the footer. To keep the nav consistent across pages, edit it everywhere it appears (the five HTML files all carry their own copy — there's no template engine).

Color tokens live in `:root {}` at the top of `css/styles.css`. Change `--accent`, `--oak-leaf`, etc. once and the whole site updates.

## Contact form

The form on `contact.html` opens the user's email client with a pre-filled message (via `mailto:`). It does not POST to a server.

To collect submissions on the server side instead, swap the form handler in `js/nav.js` for one of:

- **Formspree** (`https://formspree.io`) — replace form `action` with their endpoint.
- **Netlify Forms** — if hosting on Netlify, add `data-netlify="true"` and `name="contact"` to the `<form>` tag.
- **Cloudflare Workers / Pages Functions** — write a small POST handler.

No build step is required either way.
