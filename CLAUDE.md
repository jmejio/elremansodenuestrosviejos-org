# CLAUDE.md

## Project

Static one-page website for **El Remanso de Nuestros Viejos**, a Colombian non-profit (Marinilla, Antioquia) that runs a home for older adults. No backend, no build step, no framework. It may be maintained later by someone else, so keep it simple: plain Bootstrap 5.3 classes, green and white.

- Files: [index.html](index.html), [css/site.css](css/site.css), `assets/images/` (photos `imageN.jpg`, logo).
- Preview: open `index.html` in a browser (needs internet, Bootstrap loads from the jsDelivr CDN). There is no package manifest, linter or test suite.
- Bootstrap is linked from jsDelivr, pinned to `5.3.3`, on purpose: do not vendor it.

## Content

- `index.html` is the source of truth for the Spanish copy, and the client considers it complete. Do not rewrite, translate or invent copy; restructure and style it.
- This is a real charity: NIT, phone numbers, address and benefactor names are factual, so copy them exactly. The `mailto:` and `tel:` links must match the visible text. Preserve the user's IDE edits to the contact lines.
- Text quirks left as written: "Bienvenido!" without ¡, "Gerontólog@s", "1.988".
- Everything the visitor reads is in Spanish; code, file names and comments are in English.

## Structure to preserve

- `<html lang="es">`, one `<h1>`, a `<header>` with the navbar, `<main id="contenido">` with ten `<section>`s (`bienvenido`, `ubicacion`, `quienes-somos`, `vision`, `objeto-social`, `nuestra-sede`, `historia`, `done-aqui`, `benefactores`, `marco-legal`) and `<footer id="contactenos">`. Each section has an `<h2>` except `ubicacion` (it has `aria-label`, no navbar link) and `vision` / `objeto-social`. Those two are Quienes Somos split into their own background bands: their titles are `<h3>` (with `aria-labelledby`), and only `quienes-somos` (Misión, plus the "Quienes Somos" `<h2>`) has a navbar link. Their photos go on the right / left / right (Misión, Visión with `flex-md-row-reverse`, Objeto Social).
- The footer's top tier is three columns from `lg` (logo, Contáctenos, map) and stacks below that. The map (`#mapa`, right side) is a click-to-load facade to save mobile data: a "Ver mapa" button whose `data-src` holds the Google embed URL, and a script at the bottom of `index.html` that creates the `<iframe>` only when it is tapped. A plain "Abrir en Google Maps" link below it works without JS. Do not turn it into a plain `<iframe>`.
- Navbar links follow page order and reuse the section headings; there is no Benefactores link on purpose.
- Sections alternate `bg-primary-subtle` / `bg-white`, starting light green and ending light green at the footer. If you add or remove a section, re-check that no two neighbors match.
- Every `<img>` has Spanish `alt`, `width`/`height` and a relative path (`assets/images/imageN.jpg`). Photos below the fold use `loading="lazy"`. Photos are cropped with `ratio ratio-4x3` + `object-fit-cover`.
- Nuestra Sede (`#fotos-sede`) is a manual carousel (arrows only, no autoplay) because the user removed the Pausar button. Do not add autoplay back without a pause button (WCAG: anything that moves by itself for over 5 s needs one) and reduced-motion handling.
- The script at the bottom of `index.html` closes the phone menu after a section link is tapped. Do not replace it with `data-bs-toggle` on the links, because Bootstrap then cancels the jump to the section.

## Styling

- The green theme is in `css/site.css`, which overrides Bootstrap CSS variables and must load after `bootstrap.min.css`. To recolor, edit those variables, not individual elements. Prefer Bootstrap utilities over new custom CSS.
- Colors meet WCAG AA (4.5:1). Re-check contrast if you change one.
- Bootstrap hard-codes blue for some components (`.btn-primary`, `.btn-outline-primary`); only `.btn-outline-primary` is re-colored so far, so fix any other one before using it.
- The audience is older adults and their families: 18px base font, high contrast, tap targets of about 44 px (standalone links use `d-inline-block py-2`), phone-first.
- Never use a big gutter such as `g-5` on a `.row` inside a `.container` without a breakpoint (it causes sideways scrolling on phones); use `g-4 g-lg-5`.
- Breakpoints: the hero and footer stack until `lg`; photo-and-text rows go side by side from `md`. On phones text comes before its photo.
- The navbar must stay light because the logo has dark text.

## Logo

`assets/images/logo.png` is the client's original: keep it untouched and never on a dark or green background. The page uses `logo-sm.png` (360 px wide copy); regenerate it from `logo.png` if the logo changes.

## Open items

- Repeated images: image4, 5, 6, 10, 11 appear twice, and image18 is in both `#ubicacion` and the carousel. The user has not decided which to drop.
- Ask the client whether the third phone number in `image22.jpg` (310 444 90 58) belongs in Contáctenos. The landline "562 49-69" has no area code, so it is not a `tel:` link.
- Photos total about 4 MB; resizing to about 1200 px wide would help.
- No meta description yet (needs new copy).
- The page has not been checked in a browser. Do that before calling the design finished. Headless Chrome will not render narrower than about 500 px, so test phone widths with an `<iframe width="390">` in a scratch page.

## Workflow

Work is driven by local markdown files, never GitHub issues: `idea-to-prd` writes `issues/prd.md`, `prd-to-issues` splits it into `issues/NNN-title.md`, `issue-to-implement` builds one issue and moves it to `issues/done/`. Skills are in `.claude/skills/`. Update this file when architecture or commands change.
