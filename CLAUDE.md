# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, no-build marketing website for "RootSource" — a fictitious product (see footer disclaimer: "Fictitious product for illustrative purposes") for an agricultural-answers API/widget. There is no backend, no build tooling, no package manager, and no test suite — just HTML, one CSS file, and one small JS file.

## Running locally

There is no build step. Pages use clean URLs (`/about/`, not `/about.html`), which requires serving over HTTP rather than opening files directly — a `file://` path won't resolve the root-relative asset/nav links:

```
python3 -m http.server 8000
```

Then visit `http://localhost:8000/`.

## Structure

- `index.html` — product/home page, served at `/`
- `about/index.html` — company/origin story and team, served at `/about/`
- `contact/index.html` — contact page, served at `/contact/`
- `evaluation/index.html` — independent evaluation write-up with anchor targets (`#agribench`, `#battle-of-the-bots`, `#animal-frontiers`) linked from the home page's case-card teasers, served at `/evaluation/`
- `styles.css` — single stylesheet shared by all pages
- `script.js` — single behavior: mobile nav menu toggle (`#navToggle` / `#navLinks`)

Every page — regardless of its own path depth — links to CSS/JS/images and to other pages with root-relative paths (`/styles.css`, `/script.js`, `/images/...`, `/`, `/about/`, `/contact/`, `/evaluation/`), never relative ones. This is what lets `about/index.html` sit one directory deep and still resolve assets correctly.

All four pages share the same header/nav and footer markup verbatim (copy-paste, not templated — there's no include mechanism). When changing nav links, footer content, or the brand SVG mark, update all four HTML files identically.

## Design system (styles.css)

CSS custom properties defined once in `:root` at the top of `styles.css` drive the whole visual system — colors (soil-horizon palette: gold/rust/green tones plus a "verification ink" blue reserved for citation marks and stamps), fonts (Fraunces for display, Inter for body, IBM Plex Mono for eyebrows/mono labels), radii, and the content container width. Reuse these variables rather than hardcoding new values.

Recurring structural class patterns to follow when adding sections:
- `.wrap` — max-width content container used inside every `<section>`
- `.section-alt` / `.section-dark` — alternating background treatments between sections
- `.eyebrow` — small mono-font label above section headings
- `.strata` — decorative divider (four `<span>`s) used between major sections
- Citation markers use `<span class="cite">N</span>` inline, paired with `.source-chip` elements listing source domains

## Content conventions

- Citation-first framing is the core product message — copy changes should preserve the "cited / sourced / traceable" emphasis rather than making unqualified claims.
- Fonts are loaded from Google Fonts via a `<link>` in each page's `<head>`; keep the same weight/family list across pages if you touch it.
