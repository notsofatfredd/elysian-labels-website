# Elysian Labels — deploy notes

Static site, no build step. Edit `index.html` directly, upload via cPanel File Manager to `public_html/`.

## 2026-08-17 — ATLAS Web OS pass
- Added system-aware dark/light mode: `prefers-color-scheme` default + manual toggle (header button), choice persisted in `localStorage`, synchronous `<head>` script avoids flash-of-wrong-theme. Pinned-dark brand sections (hero panel, header/footer navy, trust bar, quote-form panel) intentionally stay dark in both modes — only page chrome (cards, borders, body text) adapts. All new text/background pairs checked against WCAG 4.5:1/3:1, not eyeballed — see contrast notes below.
- Added bottom-anchored mobile CTA bar (Request Quote), thumb-zone pattern for single-conversion-event sites.
- Added `.htaccess`: HSTS/nosniff/SAMEORIGIN/referrer-policy headers, forces HTTPS, blocks `.bak`/`.old`/`.orig` and `.md` from being served.
- **Fixed a real pre-existing bug**: a later `.nav{display:none}` rule in the responsive section was permanently overriding the earlier collapsible-dropdown nav styling, so the mobile burger menu could never actually open. Removed the stray rule.
- **Known issue, not fixed here (flag for Saeed — Identity OS decision, not Web OS)**: the brand gold (`#C69A3B`) on the light-mode page background (`#FBFAF7`) measures ~2.5:1 contrast, below the 4.5:1 WCAG AA text threshold. Used on eyebrow labels, section codes (`SEC/01` etc.), SKU tags, table labels. Pre-dates this pass — not introduced by it. Fixing it means either darkening the gold for text use or bumping size/weight past the large-text 3:1 threshold; deferred since it's a brand-color call, not implemented unprompted.
- Still open from before this pass: `og:image` tag is commented out, no `/assets/og-elysian.jpg` exists yet; no physical address on the quote form.

## 2026-08-17 (part B) — pre-client-preview sweep
- **Fixed a real, more serious pre-existing bug found on full re-read**: a stray `*/` with no matching `/*` in the `<style>` block meant CSS parsers treated the orphaned comment text plus the entire `.ph{...}` selector as one invalid rule and dropped it silently. This was the base styling (checkerboard background, border, flex layout, sizing) for every "photography pending" placeholder box — Products, Materials, Why Elysian and Insights sections all use it. Would have rendered as unstyled/collapsed boxes in front of the client. Verified fixed by counting `/*`/`*/` and `{`/`}` balance across the whole file (both even now).
- Full sweep also covered and passed: every nav anchor (`#materials`/`#industries`/`#products`/`#insights`/`#why`/`#quote`) resolves to a real id, every CSS custom property resolves to a defined token, no orphaned/undefined `var()` calls, no duplicate conflicting selectors beyond intentional responsive-breakpoint overrides, no leftover `console.log`/debug code, no lorem-ipsum or template filler text.
- **Known, not fixed — intentionally placeholder for this preview stage**: 3 "Resources" article cards and the footer Privacy/Terms links use `href="#"` (pages don't exist yet); the quote form `action="#"` has no real backend endpoint wired up yet. Normal for a preview build, but the client should know clicking those does nothing yet.

## 2026-08-17 (part C) — real product photography wired in
Saeed supplied `files (12).zip` (real photos, matching `ASSET-MANIFEST.md`) and a separate `Elysian_Labels_Samples.zip` (real photos too, sourced via WhatsApp from Sade, owner — but for a **new, different product line**: leather patches, woven/swing-tag labels, cotton labels. Not used here — that's a new site section, out of scope for this pass, follow-up planned separately).

From `files (12).zip`, 4 of the 5 "product" images were genuine photos and got wired into their matching `.ph` placeholder:
- Direct Thermal Rolls card → `elysian-prod-thermal-direct-overview.jpg`
- Thermal Transfer Rolls card → `elysian-prod-thermal-transfer-consumables.jpg`
- Materials & Adhesives section → `elysian-prod-colors-range.jpg`
- Why Elysian section → `elysian-prod-thermal-direct-detail.jpg`

**The 5th file, `elysian-prod-spec-silver-polyester.jpg`, was not a real photo** — opened it and it's just a text graphic reading "PRODUCT — SILVER POLYESTER INDUSTRIAL ROLL, METALLIC FINISH" (the placeholder's own caption text, rendered as an image). Did not use it. The Industrial Polyester Rolls card still shows the "Photography pending" placeholder — a real photo for it doesn't exist yet.

The 3 Insights/Resources article thumbnails also remain placeholders — none of the available real photos match those editorial concepts (frozen carton, scanner reading, machined part), and using a mismatched photo there would be worse than the placeholder.

New CSS: `.ph-photo` (reuses the existing `.ph--4x3`/`.ph--16x9` aspect-ratio classes, `object-fit:cover` crop) — applied directly to `<img>` tags in place of the empty `.ph` div, `loading="lazy"` on all 4.

## 2026-08-17 (part D) — new-line teaser section + AI-tell/dead-click sweep
Saeed wanted the site filled today (invoice going out), including the leather/garment line — but the copy on the empty Industrial Polyester Rolls card is specifically about waterproof/chemical-resistant industrial rolls, so a leather patch photo there would directly contradict the text next to it. Built a proper (if compact) new section instead: **"New at Elysian — Custom garment labelling"** (`id="custom-labels"`), inserted between Products and Materials. Real photo (`elysian-sample-leather-patch.jpg`, cropped from `02_Royalsa_leather_label.jpg` in `Elysian_Labels_Samples.zip` — removed a burned-in phone camera watermark and excess background, kept just the leather patch itself), honest copy pulled directly from Sade's own WhatsApp description (leather patches, woven/silk labels, swing tags, cotton labels, denim die-cut), CTA to the quote form. Industrial Polyester Rolls card and the 3 Insights thumbnails are still placeholders — still no real photo for those specifically.

Also ran the AI-tell / dead-click-affordance audit (per the standing ATLAS Web OS checklist): scanned all visible copy for generic AI-marketing vocabulary (seamless, unparalleled, cutting-edge, leverage, elevate, etc.) — none found, copy reads as genuinely specific/technical throughout. Found and fixed one real dead-click tell: `.ind` (Industries) cards had a strong hover lift + glow + shadow — the classic "this is a button" visual cue — but are plain, non-interactive `<article>` elements with zero destination, not even a `href="#"` placeholder. Softened to just a border-color highlight on hover (ambient "you're looking at this" feedback, not a false button affordance). The already-known `href="#"` cases (3 article cards, Privacy/Terms) are unchanged — those at least look like normal placeholder links, not full hover-triggered button illusions.

## 2026-08-17 (part E) — removed Industrial Polyester Rolls product + entire Resources section
Saeed's call, both fully removed rather than left as placeholders:
- **Industrial Polyester Rolls** card gone from Products (now 2 cards, "Two constructions. One standard.", grid switched `grid-3`→`grid-2`). Also stripped every other mention of polyester as an offering: meta description, hero-visual spec line, trust bar item, the quote form's Label Type dropdown, and the footer Products column.
- **Resources section** ("Notes from the line", `id="insights"`, 3 article placeholder cards) removed entirely, along with its header nav link.

Verified after: all anchor links (`#materials`/`#industries`/`#products`/`#why`/`#quote`) still resolve, no dangling `#insights` references anywhere, HTML/CSS brace and comment balance still even, `<section>`/`<article>` tag counts balanced.

## Files
- `index.html` — everything, single file, tokens documented inline at the top of `<style>`
- `.htaccess` — security headers + backup-file block
- `assets/` — logos, marks, pattern background, 5 real photos (4 barcode-label product shots + 1 cropped garment-label sample)
