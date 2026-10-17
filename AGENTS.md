# AGENTS.md

## Project

Redesign of the SH Global Solution single-page corporate site (telecom / fiber-optic infrastructure, Pasir Mas, Kelantan). Deliverable is a **single self-contained `index.html`** at repo root, intended as a PR against the original repo `github.com/fa1z0/sh-global-solution` (GitHub Pages, branch `main`).

`sh_global_solution_prd.md` is the spec of record: sections, copy (use its corrected errata, not the original's typos), asset rules, responsive behavior, and the company facts. This file records decisions the PRD does not make.

`SH GLOBAL SOLUTION.pdf` (repo root, 22 pages) is the authoritative source for company facts — history, figures (Est. 2018, 100+ staff, 200+ km, 99.9%), clientele/partners, "why choose us", and compliance claims. The PRD §7 transcribes what to use. The PDF is **untracked and gitignored** (7.4 MB, confidential): do not commit it, and read it via PDFKit if needed, e.g. `osascript -l JavaScript -e 'ObjC.import("Quartz");$.PDFDocument.alloc.initWithURL($.NSURL.fileURLWithPath("SH GLOBAL SOLUTION.pdf")).string.js'`.

## Stack (locked — no substitutions)

- **One HTML file.** Hand-written CSS in a single `<style>` block. No build step, no npm, no `package.json`. External deps are CDN-only: Satoshi (Fontshare) and IBM Plex Mono (Google Fonts). No JS libraries.
- **Fonts:** **Satoshi** via Fontshare for display/body, plus **IBM Plex Mono** via Google Fonts for small meta only (section indices, stat units, field labels). Do not swap Satoshi.
- **Hero:** a **blueprint-grid backdrop** (CSS `linear-gradient` grid at `48px`, faded to `--bg` by a radial `::after`) that is **pinned and scroll-scrubbed** — the grid zooms while the headline and copy recede (`--p` 0→1 over a 150vh hero). Still no Three.js/SVG and no motion library; the hero itself stays a scroll scrub, never an autoplay animation.
- **Languages:** **English (default) + Bahasa Malaysia.** All translatable copy lives in the `I18N` dictionary keyed by `data-i18n` attributes in the markup; the `EN | BM` toggle swaps text and persists `lang` in `localStorage`. Write real, proof-read copy in both — never machine-translate strings in place.
- **Themes:** **light + dark, dark-first** (`<html data-theme="dark">`). All colour comes from CSS custom properties on `:root` (dark) overridden by `html[data-theme="light"]`; never hard-code colours in rules.
- **Vanilla JS, inline**, for: language toggle, mobile hamburger, theme toggle (persisted), mega-menu / mobile overlay, and back-to-top. Smooth scrolling is CSS. No other libraries.
- Motion is **one system: the route**. A rAF-throttled vanilla-JS scroll handler writes two custom properties — `--p` (hero scrub 0→1) and `--route` (page progress 0→1) — and CSS consumes them. Everything is gated on `html.motion`, a class the head script adds only when `prefers-reduced-motion` is not `reduce` *and* `IntersectionObserver` exists; when it is absent the route bar and rail are hidden and all content is visible from first paint, so reduced-motion and no-JS visitors get a fully static page. The pieces: the pinned hero scrub; the sticky why-us panel; a fixed route strand under the nav plus a waypoint rail (≥1280px) with one node per numbered section; a "signal reveal" (`.sig` observed by IntersectionObserver, staggered within a group via `--i`) in which each section's fibre bar draws itself in segment by segment; parallax inside `.par-frame`; count-ups on `[data-count]`; and two ambient loops (a pulse on the route head, a ring on the current waypoint). Every effect belongs to the one route idea, so the page reads as a single instrument rather than scattered fade-ups — which is also the skill's objection to "motion on every section". Never pin an element with `overflow: hidden` on an ancestor — it silently disables `position: sticky`.

## Assets

- Images that already exist in the target repo are referenced by **relative path**: `images/logo.png`, `images/2149212625.jpg` (mission), `images/bosmi.jpeg` (Syahmi Halim). They survive the PR because `images/` stays upstream.
- Org-chart portraits are **committed local files** (the client supplied them, so no more Unsplash faces except Planner Azrul, who has no photo): `images/ceo.jpeg` (Mohd Lutpi), `fahmi.jpeg` (Fahmi Izzudin), `azhan.jpeg` (Mohd Azhan), `faris-farhan.jpeg` (Mohd Faris), `nurul-huda.jpeg` (Nurul Huda), `nor-asiah.jpeg` (Nor Asiah, Procurement Manager — the profile's "Hanis Halim" was superseded by the client), `adam-fitri.jpeg` (Adam Fitri). Kebab-case filenames only; `.gitignore` lists the three preview copies individually so `images/` itself is committable.
- Everything else is **hotlinked Unsplash** URLs (reuse the original's where already professional). Never base64-inline images — it bloats the file and makes the PR diff unreadable.
- Joke team photos in the original (Patrick Star, Ronaldo, celebrity images) are spec'd replaced with neutral Unsplash portraits.
- The Google Maps iframe in the contact section is real, not a placeholder — copy the embed URL from the original `index.html`.

## Design

Load `.agents/skills/frontend-design/SKILL.md` for any UI work; its anti-generic-design constraints apply. Follow its two-pass process: brainstorm tokens (palette, Satoshi type scale, layout, principles) and self-review against its "generic AI design tells" **before** writing code. Do not edit the skill files or `skills-lock.json` (hash mismatch).

Design direction: **unifi-like structure + Linear/Raycast craft.** Palette is drawn from `SH GLOBAL SOLUTION.pdf` — a deep **teal-navy** base, a **cyan accent** (`#00B0C0`), and an **orange secondary** (`#E06020`); light mode maps the same hues onto white/cool-grey. Keep the fiber color-code as the recurring motif, use hairline borders over drop-shadows, and avoid ALL-CAPS labels and em-dash eyebrow labels.

The **Malaysian identity is carried in copy only** (e.g. "for a connected Malaysia", the footer "Registered in Malaysia · Pasir Mas, Kelantan" line) — earlier red-over-blue rule / "Proudly Malaysian" motifs were removed by the client; do not reintroduce flag or rule decorations.

Chrome: sticky nav — About / Services (mega-panel) / Why us / Quality / Contact, plus an EN|BM toggle, a theme toggle, and a "Request a quote" CTA; full-screen slide-in menu on mobile; a multi-column footer with a back-to-top control.

Nav links depend on the anchor IDs `#services`, `#team`, `#qshe`, `#milestones`, `#contact` — never rename these. The page is **deliberately compact** (9 sections): hero, clients/partners strip, about (background + mission merged), services, why-us (+ delivery stats merged), team, quality & safety (compliance + QSHE merged), milestones, contact. The original's teamwork gallery and separate core-values/mission/summary sections were cut; the PRD §3 lists the order.

## Content corrections

The original copy has known typos and quirks (e.g. "provate enterprises", "shglobalsoution28@gmail.com", joke footer, WhatsApp link to a placeholder number). The PRD's errata section is the corrected source of truth — apply those fixes, don't copy the original verbatim.

Trust content (certs, clients, SLA figures) comes from `SH GLOBAL SOLUTION.pdf` — never invent credentials or client logos. The profile's text layer lists clientele/partner *categories*, but its p.14 "Previous Project" table (an embedded image) names real clients and all 45 contracts — the site uses those names, with official logo files for the six public brands (see PRD §7) — and **no license numbers**, so no CIDB/SKMM/ISO badges exist to show.

## Verification

No lint, test, or CI tooling exists — verification is manual. Preview with live reload:

```sh
./serve.sh          # browser-sync, http://localhost:4173, auto-reloads on save
```

`serve.sh` is dev-only tooling (uses `npx browser-sync`; no npm files are committed). Port overridable via `PORT=… ./serve.sh`. Do **not** default to 8080 — it is commonly occupied (an `ssh` tunnel here holds it).

Only the three preview copies are gitignored (listed individually in `.gitignore`); the org portraits are committed. Fetch the preview set once for local viewing:

```sh
mkdir -p images && cd images
for f in logo.png 2149212625.jpg bosmi.jpeg; do
  curl -sSo "$f" "https://fa1z0.github.io/sh-global-solution/images/$f"
done
```

Check at 1440 / 768 / 390px in **both themes**: sticky nav, hamburger, mega-menu and mobile overlay, theme toggle + persistence, anchor scrolling, no horizontal overflow, no console errors, images loading (hotlinked Unsplash needs network), reduced-motion emulation. The computer-use GUI skill has no macOS provider here — drive headless Chrome over the DevTools Protocol instead (Node 24 has a global `WebSocket`; no packages needed).

To sample colours out of the PDF without extra tooling, render pages with PDFKit in JXA and read the BMP pixels (see the PDF note above); `page.thumbnailOfSizeForBox` and `NSBitmapImageRep.representationUsingTypeProperties` are the bridge names that work.

## Git & PR flow

- `main` is the working branch. Repo-local git identity is a placeholder — amend if needed.
- Ship flow: `gh repo fork fa1z0/sh-global-solution --clone=false`, then in a **temp clone of the fork** (never pollute this repo's history into the PR branch): branch from upstream `main`, copy in **only** `index.html`, commit, push, `gh pr create --repo fa1z0/sh-global-solution`.
