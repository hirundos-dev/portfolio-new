# Portfolio — Static HTML/CSS/JS Website

Personal portfolio site for product designer Andrei Lynnik. Showcases 6 design projects with case studies, galleries, and video embeds.

**Stack:** HTML5, CSS3 (vanilla, custom properties, native nesting), Vanilla JS (ES6+, Web Components), PhotoSwipe 5.

**Deploy:** GitHub Pages (`hirundos-dev.github.io/portfolio/`). No build step — raw static files.

## Operating Principles

### Communication
- Start with the result; keep evidence and caveats; remove preamble and repetitions.
- Do not agree automatically: justify errors or weak solutions and offer alternatives.
- Complex changes: explain through product effect, trade-offs, and risks first, then briefly through implementation and verification.

### Autonomy
- Answers, explanations, reviews, diagnostics, and plans do not permit changes; requests to change, implement, or fix permit local reversible actions and relevant checks within the task scope.
- Ask before external, destructive, irreversible, or paid actions, publishing, deployment, secret disclosure, or significant scope expansion.

### Code & Tools
- Determine project tools from configuration, documentation, and lock files; do not silently substitute a missing runner.
- Preserve unrelated user changes; do not apply destructive git commands or kill processes unrelated to the task.

## Fix Protocol
Every code change must follow `docs/safe-fix.md` — root cause analysis, minimal change, self-check, blast zone verification.

### safe-fix — ABSOLUTELY MANDATORY
**Violation = revert.** Before every code change, answer, diagnosis, or plan involving the codebase:
1. Phase 1 — Root Cause (read affected files; for bugs — find cause via code, config, or logs; separate confirmed facts from assumptions)
2. Phase 2 — Minimal fix
3. Phase 3 — Self-check (verify result proportional to risk; explicitly state completed checks, unchecked items, and remaining risks)
4. Phase 4 — Record in `docs/history.md`

**No Phase 1–4, no line of code. No Phase 1–4, no answer claiming work is done.** Changelog not updated — fix not complete.

Applies to: code changes, refactors, configuration edits, documentation updates, dependency changes, and any response that claims a task is complete.

## Change History
All changes and known issues are documented in `docs/history.md` and `docs/backlog.md`. Read both at the start of every session — they preserve context between conversations.

## Current Status
See `docs/history.md` for past changes and `docs/backlog.md` for open issues.

## Project Structure

```
portfolio/
├── index.html              # Main page: user card, project grid, footer
├── pages/                  # Project case study pages (6 subdirectories)
│   ├── bcs-1/              # BCS MI — Technical Analysis
│   ├── bcs-2/              # BCS MI — Currency Onboarding
│   ├── med-1/              # Medreg 39 — Doctor appointment portal
│   ├── nda-1/              # RentCraft — Trucking classifieds
│   ├── finance/            # grecha — Mobile finance tracker
│   └── palka/              # Palka UI — Design system components
├── css/                    # Stylesheets
│   ├── normalize.css       # Reset (v8.0.1)
│   ├── colors.css          # Color tokens (CSS custom properties)
│   ├── reset.css           # Box-sizing, body, forms
│   ├── settings.css        # @font-face, fluid typography, density
│   ├── layout.css          # Wrapper, grid, spacing, squircle
│   ├── ids.css             # Typography + component styles (main DS file)
│   ├── navbar.css          # Sidebar navigation
│   ├── promo-block.css     # Inline gallery
│   ├── footnotes.css       # Footnote popups
│   └── gallery.css         # PhotoSwipe lightbox
├── js/                     # JavaScript
│   ├── observer.js         # IntersectionObserver for scroll animations
│   ├── emerge.js           # Third-party lazy-load animation
│   ├── gallery.js          # Web Component: <ids-gallery>
│   ├── footnotes.js        # Web Components: <ids-footnote-link>, <ids-footnote>
│   ├── promo-block.js      # Pointer-move inline image carousel
│   └── lib/                # Third-party: PhotoSwipe 5, version, weather
├── fonts/                  # Onest (active), PT Root UI (available)
├── data/img/               # Main page assets (avatar, covers, favicon)
└── docs/                   # Documentation
```

## Architecture

### Static Site — No Build System
- Raw HTML5, CSS3, and vanilla JS served as-is.
- No package.json, no bundler, no transpiler, no minification pipeline.
- All CSS and JS are hand-written source files.
- Deployed via GitHub Pages from `main` branch.

### CSS Architecture — IDS (Interface Design System)
- **Naming:** `ids__` namespace prefix for design system classes.
- **Custom Properties:** semantic color tokens (`--ids__*`), density (`--ids__density: 1.3`), radius (`--ids__radius: 1.5em`).
- **Native CSS Nesting** throughout (Chrome 120+, Firefox 117+).
- **Fluid Typography:** `calc()` with viewport units, 3 breakpoints (320/768/1500px).
- **Responsive:** `@media (width < 768px)` range syntax (Chrome 104+, Firefox 113+).
- **Squircle corners:** native `corner-shape: squircle` with SVG mask fallback.
- **Single light theme** — no dark mode.

### JavaScript Architecture — Web Components
- **`IdsGallery`** (`<ids-gallery>`) — wraps PhotoSwipeLightbox for image lightbox.
- **`IdsFootnoteLink`** + **`IdsFootnote`** + **`FootnoteStore`** — interactive footnote system with numbered badges, popups, mobile bottom-sheet.
- **`observer.js`** — IntersectionObserver toggles `sleep` class on `.ids__snooze` elements.
- **`promo-block.js`** — pointer-move image switching with dot/dash indicators.
- No frameworks, no jQuery, no build step.

### Pages
- Each project page under `pages/<name>/index.html` follows the same template: back button, cover image, context/what-I-did/result sections, embedded videos/images.
- Pages load a subset of JS (observer + optional emerge/gallery/footnotes).
- Assets stored in `pages/<name>/assets/` (PNG covers, screenshots, MP4 videos, GIFs).

## Coding Conventions

### HTML
- Semantic HTML5 (`<header>`, `<main>`, `<footer>`, `<section>`, `<figure>`).
- Russian language (`lang="ru"`) throughout.
- Back button pattern in project pages: `<a href="../../">` with SVG arrow.
- Videos: `<video loop muted playsinline autoplay>` for ambient content.

### CSS
- Follow existing `ids__` naming convention strictly.
- Use CSS custom properties from `colors.css` — no hardcoded color values.
- Use fluid typography via `settings.css` variables — no hardcoded `px` font sizes.
- Use spacing utilities from `layout.css` (`.ids__space S/M/L/XL`).
- Prefer `rem`/`em` over `px` for sizing.
- Test at 320px, 768px, and 1500px breakpoints.
- No CSS preprocessors, no PostCSS, no utility frameworks.

### JavaScript
- Vanilla ES6+ only — no frameworks, no jQuery, no build step.
- Web Components for reusable UI (`customElements.define()`).
- No `console.log` in production code.
- Progressive enhancement — site must work without JS (scroll animations degrade gracefully).

### Images
- PNG for screenshots and covers, MP4 for video demos.
- Cover images in `data/img/cover-*.png` (main page) or `pages/*/assets/cover.png`.
- PhotoSwipe for lightbox galleries.

## Browser Targets
- Modern browsers with CSS nesting support (Chrome 120+, Firefox 117+, Safari 17.2+).
- `:has()` selector, range media queries, CSS custom properties required.
- No IE11, no legacy edge.

## Banned Practices
- No new dependencies without discussion.
- No CSS preprocessors (Sass, Less, PostCSS).
- No JavaScript frameworks or build tools.
- No hardcoded colors — use tokens from `colors.css`.
- No hardcoded font sizes — use fluid typography from `settings.css`.
- No `!important` except for third-party overrides.
- No inline styles on structural elements.
- One change = one purpose.
- Destructive git commands outside task scope (rebase --force, reset --hard, push --force).

## Improvement Proposals
Follow the template in `docs/improvement.md` — Context & Problem,
Proposed Solution, Considered Alternatives, Risks, Implementation Plan.
