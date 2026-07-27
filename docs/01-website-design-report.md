# KairosWeb — Website Design Report

**Date:** 2026-07-27  
**Framework:** Astro 7  
**Typography:** Geist Sans  

---

## Overview

KairosWeb is a single-page brand presence website built with Astro. The site serves as a digital identity for "Kairos" — a concept rooted in the ancient Greek idea of the opportune moment.

The design follows the Vercel brand guidelines for report websites (see `docs/instruction/design.md`) but adapts them to a personal brand context with stricter chromatic constraints.

---

## Design Constraints

| Constraint | Implementation |
|---|---|
| No chromatic colors | Palette strictly #000, #fff, and #666 for secondary text |
| No hover effects | Zero `:hover` rules; only `:focus-visible` for keyboard accessibility |
| No borders or bar cards | No `border`, `outline` (except focus), or card containers |
| No emojis | No emoji characters anywhere |
| Scroll-based effects only | Color inversion, fade-in, and zoom driven by scroll position |

---

## Scroll Effects

### Color Inversion

Sections strictly alternate between light (`background: #000`, `color: #fff`) and dark (`background: #fff`, `color: #000`). As the user scrolls, the page rhythmically inverts, creating a strong visual pulse without any chromatic color.

The fixed header uses `mix-blend-mode: difference` with white text, causing it to automatically invert against each section's background — white on dark sections, black on light sections.

### Fade-In

Content sections use Intersection Observer to detect when they enter the viewport (`threshold: 0.2`). On intersection, a CSS transition animates `opacity: 0 → 1` and `transform: translateY(40px) → translateY(0)`. Each element animates once and is then unobserved.

### Zoom

The hero title (`"KAIROS"`) responds to scroll position with a subtle scale transform (`1.0 → 1.08`) and opacity fade (`1.0 → 0.0`). The effect is driven by a passive scroll event with `requestAnimationFrame` for smooth 60fps performance.

### Reduced Motion

Uses `prefers-reduced-motion: reduce` media query to disable all animations. Fade-in elements start fully visible, `scroll-behavior` is set to `auto`, and the zoom scroll listener is not registered.

---

## SEO & Web Standards

| Feature | Implementation |
|---|---|
| Title & meta description | Descriptive `<title>` and `<meta name="description">` |
| Open Graph | `og:title`, `og:description`, `og:url`, `og:type` |
| Twitter Card | `twitter:card`, `twitter:title`, `twitter:description` |
| Canonical URL | `<link rel="canonical">` |
| Structured data | JSON-LD `WebSite` schema injected via `<script type="application/ld+json">` |
| Semantic HTML | `<main>`, `<section>`, `<header>`, `<footer>`, `<nav>`, `<h1>`, `<h2>` |
| Language | `<html lang="en">` |

---

## Accessibility

| Feature | Implementation |
|---|---|
| Skip link | Visible on keyboard focus, jumps to `#main` |
| Landmarks | `role="banner"`, `role="contentinfo"`, `nav` with `aria-label` |
| Heading hierarchy | Single `<h1>`, visually hidden `<h2>` per section |
| ARIA labels | `aria-label` on hero, `aria-labelledby` on sections |
| Decorative hiding | Section numbers use `aria-hidden="true"` |
| Visible focus | `:focus-visible` with 2px solid outline on nav and skip link |
| Color contrast | Pure black on white / white on black — exceeds WCAG AAA |
| Screen reader text | `.visually-hidden` class for headings visible only to assistive tech |

---

## Project Structure

```
/
├── public/
│   └── favicon.svg
├── src/
│   └── pages/
│       └── index.astro
├── docs/
│   ├── instruction/
│   │   └── design.md
│   └── 01-website-design-report.md
├── astro.config.mjs
└── package.json
```

All content, styles, and client scripting are contained in `src/pages/index.astro`. Styles are scoped via Astro's built-in CSS scoping (`data-astro-cid-*` attributes). Client scripting uses vanilla JavaScript with `IntersectionObserver` and `requestAnimationFrame`.

---

## Build

```
Astro v7.1.3
Output: static
Pages built: 1 (index.html)
Build time: ~13s
Size: ~4KB (HTML + inlined CSS + inlined JS)
```

The site compiles to a single static HTML file with all CSS and JS inlined. No external runtime dependencies. No JavaScript frameworks shipped to the browser.
