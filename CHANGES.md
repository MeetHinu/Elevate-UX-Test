# UX & accessibility changes (test copy)

Based on live commit `268bba2` (Netlify deploy `6a6839168c6a82000853ed23`).
The baseline is tagged `pre-ux-changes` in the working copy this was built from.

## Measured results (Chromium, production build)
| | Before | After |
|---|---|---|
| Portfolio page images | 18.2 MB | 3.8 MB |
| Home page images | 2.1 MB | 1.4 MB |
| `public/images` on disk | 20.9 MB | 4.9 MB |
| axe-core WCAG 2.1 AA (5 pages) | 36 contrast failures | 0 violations |
| Unit tests | 29 | 36 (all passing) |

## What changed
**Accessibility**
- Text brass `#A9834F` (3.27:1) -> `--accent-brass-text` `#7D5C2E` (5.75:1). Bright brass kept for decoration.
- CTA band sage `#8A9482` (2.98:1) -> `#58634F` (5.98:1). Dim body text .62 -> .68 opacity.
- Mobile menu toggle: text glyph -> SVG, 44x44 target, Escape closes, `aria-controls`.
- Mobile nav links ~44px tall. Visible `:focus-visible` rings on links, buttons, filters.
- Skip-to-content link and a `<main>` landmark. Sticky-nav-safe scroll padding for focused fields.
- Homepage now has an `<h1>` (visually hidden; the design is unchanged).
- Decorative hero/contact photos use `alt=""`; the two generic alts were replaced.

**Contact form**
- Visible "Thank you" confirmation (focus moves to it) instead of only the button text changing.
- On a failed submit, focus jumps to the first invalid field and an error summary appears.
- "Project type", "timeline" and "how did you hear" start on "Select…" (no more everyone = Instagram).
- Phone accepts AU mobiles, AU landlines and international numbers. `autocomplete` attributes added.
- Address field is now a proper combobox: arrow keys, Enter to choose, Escape to close, screen-reader announcements.

**Performance**
- `npm run optimize-images` regenerates 1800px-max WebP from `images-original/` (same sharpen/contrast look).
- `.jpg` references -> `.webp`; async decoding, lazy loading below the fold, priority hint on the home hero.

**SEO**
- Per-page `<title>` and meta description. Open Graph / Twitter tags, canonical, `og-image.jpg` (1200x630).
- `InteriorDesigner` JSON-LD (name, phone, email, hours, Instagram).

**Small things**
- Phone shown as `0402 601 808`. Removed hover-zoom on non-clickable portfolio cards. Removed unused `.founder-panel` CSS.

## Not changed (worth knowing)
- Still a client-rendered SPA: crawlers that don't run JavaScript see an empty page. Prerendering would fix that.
- No lightbox / project detail pages. No static-asset cache headers.

## Testing this copy safely
1. Push this folder to a NEW GitHub repo and import it as a NEW Netlify site. Do not attach elevatelivingstudio.com.au to it.
2. Form submissions on the test site land in that site's own Forms dashboard (no email notification unless you add one).
3. The canonical/OG URLs point at the production domain on purpose, so the test site isn't treated as a duplicate.

## Going back
- The live site is never touched by the test. To abandon: delete the test Netlify site and repo.
- If you later merge into the original repo and want out: Netlify -> Deploys -> open deploy
  `6a6839168c6a82000853ed23` -> "Publish deploy" (instant rollback), or `git revert` / reset to `268bba2`.
