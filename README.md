# Elysian Labels — deploy notes

Static site, no build step. Edit `index.html` directly, upload via cPanel File Manager to `public_html/`.

## 2026-08-17 — ATLAS Web OS pass
- Added system-aware dark/light mode: `prefers-color-scheme` default + manual toggle (header button), choice persisted in `localStorage`, synchronous `<head>` script avoids flash-of-wrong-theme. Pinned-dark brand sections (hero panel, header/footer navy, trust bar, quote-form panel) intentionally stay dark in both modes — only page chrome (cards, borders, body text) adapts. All new text/background pairs checked against WCAG 4.5:1/3:1, not eyeballed — see contrast notes below.
- Added bottom-anchored mobile CTA bar (Request Quote), thumb-zone pattern for single-conversion-event sites.
- Added `.htaccess`: HSTS/nosniff/SAMEORIGIN/referrer-policy headers, forces HTTPS, blocks `.bak`/`.old`/`.orig` and `.md` from being served.
- **Fixed a real pre-existing bug**: a later `.nav{display:none}` rule in the responsive section was permanently overriding the earlier collapsible-dropdown nav styling, so the mobile burger menu could never actually open. Removed the stray rule.
- **Known issue, not fixed here (flag for Saeed — Identity OS decision, not Web OS)**: the brand gold (`#C69A3B`) on the light-mode page background (`#FBFAF7`) measures ~2.5:1 contrast, below the 4.5:1 WCAG AA text threshold. Used on eyebrow labels, section codes (`SEC/01` etc.), SKU tags, table labels. Pre-dates this pass — not introduced by it. Fixing it means either darkening the gold for text use or bumping size/weight past the large-text 3:1 threshold; deferred since it's a brand-color call, not implemented unprompted.
- Still open from before this pass: `og:image` tag is commented out, no `/assets/og-elysian.jpg` exists yet; no physical address on the quote form.

## Files
- `index.html` — everything, single file, tokens documented inline at the top of `<style>`
- `.htaccess` — security headers + backup-file block
- `assets/` — logos, marks, pattern background
