# Backlog — Planned improvements & known issues

All items follow the template from `docs/improvement.md`.

## Status reference
- **quick-fix** — < 15 minutes, low risk
- **medium** — 30–60 minutes, medium risk
- **deferred** — needs a separate session (trigger specified)
- **wontfix** — conscious decision not to do
- **done** — completed

---

## 1. Performance & SEO

### 1.1 Missing meta tags and Open Graph
**Status:** done
**Source:** best practice

**Problem:** No `<meta name="description">`, no Open Graph tags (`og:title`, `og:description`, `og:image`), no Twitter Card tags. Social sharing shows raw URL without preview.

**Fix:** Add meta tags to `index.html` and each project page. Use existing `data/img/social.png` as `og:image`.

---

### 1.2 No sitemap.xml or robots.txt
**Status:** quick-fix
**Source:** SEO

**Problem:** No `sitemap.xml` for crawlers, no `robots.txt`. GitHub Pages may auto-generate basics, but explicit files give better control.

**Fix:** Create `sitemap.xml` with all 7 pages. Create `robots.txt` allowing all crawlers.

---

### 1.3 Unoptimized images
**Status:** done (partial — WebP done, lazy loading done; PNG fallback and `<picture>` not needed)
**Source:** performance

**Problem:** PNG covers and screenshots are large (cover-2.png is 1.9MB, cover-5.png is 2.6MB). No WebP/AVIF variants. No `loading="lazy"` on below-fold images.

**Fix:** Convert covers to WebP with PNG fallback. Add `loading="lazy"` to project page images. Consider `<picture>` element for art-directed responsive images.

---

### 1.4 No critical CSS inlining
**Status:** deferred
**Source:** performance

**Problem:** 11 CSS files loaded sequentially. Above-fold content renders after all CSS downloads.

**Fix:** Inline critical CSS (first viewport) in `<style>` tag, defer non-critical. Requires identifying critical path per page.

---

## 2. Accessibility

### 2.1 Missing alt text on images
**Status:** medium
**Source:** a11y

**Problem:** Many `<img>` tags have empty or generic alt text. Cover images in project pages lack descriptive alternatives.

**Fix:** Audit all `<img>` tags. Replace empty `alt=""` with descriptive text for meaningful images. Keep `alt=""` only for decorative images.

---

### 2.2 Keyboard navigation for interactive components
**Status:** medium
**Source:** a11y

**Problem:** `promo-block.js` (pointer-move gallery) and footnote popups are pointer-only. No keyboard equivalents for switching images or opening/closing footnotes.

**Fix:** Add `keydown` handlers for arrow keys in promo-block. Ensure footnote links are focusable and popups close on Escape (check if already implemented).

---

### 2.3 Low contrast on footer links
**Status:** quick-fix
**Source:** a11y

**Problem:** Footer social links may have insufficient contrast ratio against background (depends on gradient).

**Fix:** Verify contrast with browser dev tools. Adjust if below 4.5:1 for normal text.

---

## 3. Content & UX

### 3.4 Duplicate `lib/` and `images/` directories
**Status:** done
**Source:** cleanup

**Problem:** `lib/` was a dead duplicate of `js/lib/`. `images/` was an unused old gallery structure.

**Fix:** Both deleted (2026-07-22).

---

### 3.5 Dark mode
**Status:** deferred
**Source:** UX

**Problem:** Single light theme only. No `prefers-color-scheme` media query. Users in dark environments get bright white page.

**Fix:** Define dark color tokens in `colors.css`. Add `@media (prefers-color-scheme: dark)` override. Test all components. Deferred — requires significant token audit across IDS.

---

### 3.6 Mobile responsive audit
**Status:** medium
**Source:** UX

**Problem:** Primary breakpoint is 768px. No explicit testing documented for 320px–768px range. Project pages with video embeds may overflow on small screens.

**Fix:** Audit all pages at 320px, 375px, 414px widths. Fix any overflow, text truncation, or touch target issues.

---

## 4. Code Quality

### 4.1 `opencode.json` references removed `.opencode/`
**Status:** medium
**Source:** cleanup

**Problem:** `opencode.json` references `.opencode/prompts/agents/*.txt` and `.opencode/commands/*.md` which were deleted. Commands like `/plan`, `/code-review` will fail.

**Fix:** Either recreate `.opencode/` with portfolio-relevant agents/commands, or simplify `opencode.json` to remove broken references.

---

### 4.2 Version duplication
**Status:** done (version.js not loaded from any HTML — dead code, same as observer.js)
**Source:** cleanup

**Problem:** `js/lib/version.js` shows "2.0.1 27.04.26" but no HTML references a version display. The version text is injected into `.version-text` elements which don't exist in current HTML.

**Fix:** Either add version display to footer or remove `version.js` and its reference.

---

### 4.3 No `.gitignore` at root
**Status:** quick-fix
**Source:** best practice

**Problem:** No root `.gitignore`. `.opencode/.gitignore` exists but only covers node_modules (irrelevant now).

**Fix:** Create root `.gitignore` with common patterns (`.DS_Store`, `Thumbs.db`, editor files).

---

## 5. Infrastructure

### 5.1 No CI/CD
**Status:** deferred
**Source:** deployment

**Problem:** No GitHub Actions, no automated checks. Changes deployed manually via GitHub web UI ("Add files via upload").

**Fix:** Add GitHub Actions workflow for HTML/CSS/JS validation (html-validate, stylelint, eslint). Auto-deploy on push to main.

---

### 5.2 Git history is "Add files via upload"
**Status:** wontfix
**Source:** git

**Problem:** All commits are "Add files via upload" or "Delete X directory" — no meaningful messages. Project edited via GitHub web UI.

**Why:** This is the owner's workflow. Enforcing commit conventions would require changing the editing process, which is outside scope.
