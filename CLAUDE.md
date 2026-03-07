# CLAUDE.md — Allotment Planner

## Project Overview

A single-page Progressive Web App (PWA) providing a week-by-week planting calendar for a 2026 allotment season. Designed for four raised beds (2.4m x 1.2m) on heavy clay soil using no-dig methods.

**Deployed on GitHub Pages** at `https://<username>.github.io/<repo-name>/`

---

## Architecture

This is a **zero-dependency, pure HTML/CSS/JavaScript** project. There is no build system, no package manager, no npm, no framework.

### File Structure

| File | Purpose |
|------|---------|
| `index.html` | The entire application — HTML structure, all CSS (`<style>`), all JavaScript (`<script>`) |
| `manifest.json` | PWA manifest (app name, icons, theme colour, display mode) |
| `sw.js` | Service worker — caches all assets for offline use (cache name: `allotment-2026-v1`) |
| `icon-192.png` | Home screen icon (192x192) |
| `icon-512.png` | Splash screen icon (512x512) |
| `.nojekyll` | Prevents GitHub Pages from processing with Jekyll |

Everything lives in `index.html`. Do not split it into separate files unless explicitly asked.

---

## Application Structure (index.html)

### Sections (Tab Navigation)

The app has eight tabs, each a `<div id="..." class="section">`. Only the active section has the `.active` class (shown via CSS `display:block`).

| Tab ID | Label | Content |
|--------|-------|---------|
| `calendar` | Month-by-Month | Monthly task cards from Feb–Oct |
| `progress` | 📊 Progress Tracker | Actual vs planned progress table, windowsill occupancy, upcoming actions |
| `gantt` | Week-by-Week Gantt | Gantt chart (table view for desktop, card view for mobile) |
| `beds` | Bed Layouts | Visual diagrams of all 4 beds |
| `succession` | Succession Detail | Full table of every sowing/transplanting date |
| `checklist` | Weekly Checklist | Week-by-week interactive checkbox sowing guide |
| `guide` | 🌿 Growing Guide | Crop-by-crop expandable instructions (seed to harvest) |
| `advice` | Growing Advice | Advice cards on techniques |

### JavaScript Functions

```javascript
// Switch between main navigation tabs
function showSection(id, btn) { ... }

// Toggle Gantt chart between table and card view
function ganttView(mode, btn) { ... }

// Set colour theme ('garden' | 'slate' | 'terracotta') and persist to localStorage
function setTheme(name, btn) { ... }
```

Three self-invoking init functions also run on page load:
- `initTheme()` — restores saved theme from `localStorage`
- `initGantt()` — highlights current week column (amber); wires click-to-highlight (green) on week headers and data cells
- `initChecklists()` — restores all checkbox states from `localStorage` and saves on every change

All data is hardcoded in HTML. Persistent state (theme, checkbox ticks) is stored in `localStorage` only — no backend required.

### CSS Custom Properties (Design Tokens)

Defined in `:root` in `<style>`:

```css
--earth, --earth-light       /* Browns */
--leaf, --leaf-mid, --leaf-light  /* Greens */
--cream, --warm-white, --soil    /* Backgrounds */
--indoor, --indoor-bg        /* Sow indoors: amber */
--transplant, --transplant-bg    /* Transplant: green */
--outdoor, --outdoor-bg      /* Direct sow: blue */
--harvest, --harvest-bg      /* Harvest: red */
--chit, --chit-bg            /* Chit: purple */
--border, --text, --text-muted
--shadow, --shadow-lg
```

### Responsive Behaviour

- Mobile (max 700px): Gantt table is hidden; card view shown automatically. Nav tabs compact.
- Desktop (min 701px): Gantt card view is hidden; toggle group hidden. Table view shown.

---

## Data: Beds and Crops

### Bed 1 — Alliums, Roots & Strawberries
Layout north → south (as planted):
- Onion sets (Sturon) — plant 9 Mar, harvest Jul/Aug
- Leeks (Musselburgh) — **3 indoor sowings** (Batch 2 missed): Batch 1 sown 14 Feb (standard tray), Batch 3 sown 6 Mar (deep root trainer), Batch 4 due 23 Mar (deep root trainer). Transplant May–Jul into garlic/onion space. ~62 leeks total.
- Carrots (Chantenay Royal) — 4 succession sowings Apr–Jul, direct sow through cardboard
- Marigolds (Mango Tango) — companion between carrots and strawberries
- Strawberries (Cambridge Favourite) — plant Feb/Mar
- Garlic (Germidour) — already in ground (south end), harvest Jul
- Nasturtiums (Cherry Rose Jewel, Tip Top Pink Blush, Tom Thumb) — all 4 corners, trap crop

### Bed 2 — Potatoes, Beetroot & Roots
*(was Bed 3 before 07 Mar 2026 renumbering)*
- 1st Early Potatoes (Casablanca) — chit started ~15 Feb (18 seed potatoes), plant 23 Mar
- 2nd Early Potatoes (Charlotte) — chit started ~20 Feb (18 seed potatoes), plant 6 Apr
- Maincrop Potatoes (Setanta Organic) — chit started 6 Mar (18 seed potatoes), plant 20 Apr
- Spinach (Medania) — between potato rows
- Beetroot (Boldor) — south end, 5 succession sowings Apr–Jul
- Radish (French Breakfast) — alongside beetroot, 5 sowings Apr–Jun
- Nasturtiums — alongside potatoes

### Bed 3 — Brassicas & Shallots
*(was Bed 2 before 07 Mar 2026 renumbering)*
- PSB / Purple Sprouting Early — transplant Jun/Jul, stays until spring 2027
- Shallot sets (Biztro) — plant 9 Mar
- Cauliflower (All Year Round) — transplant 11 May, net immediately
- Spinach & Lettuce inter-crop — between brassicas May/Jun
- Nasturtiums (Tom Thumb) — all 4 corners (essential for brassica aphid control)

### Bed 4 — Tomatoes, Leafy Greens & Herbs
- Tomatoes x3: Moneymaker, Black Opal, Gardeners Delight — sown 24 Feb (72 seeds total, 24/variety, over-seeded 2.4×). Transplant best 7–8 plants 26 May (north end). Gift remaining ~12 plants.
- Basil (Greek) — between tomatoes from Jun
- Marigolds (Spanish Brocade) — between tomatoes
- Radish (French Breakfast) — at tomato base May/Jun (2 sowings)
- Chard (Rainbow) — transplant May, 2nd direct sow Jul for overwintering
- Lettuce (Little Gem) — south end, 12 succession sowings Mar–Aug (every 2 weeks)
- Spinach (Medania) — south end, succession Mar–May and late Aug

---

## Succession Sowing Summary

| Crop | # Sowings | Frequency | Season |
|------|-----------|-----------|--------|
| Lettuce | 12 (3 indoor + 9 direct) | Every 2 weeks | Mar–Aug |
| Spinach | 6 | Every 3 weeks | Mar–May + late Aug (AVOID July) |
| Carrots | 4 | Every 3–4 weeks | Apr–Jul |
| Beetroot | 5 | Every 3 weeks | Apr–Jul |
| Radish | 7 (5 Bed 2 + 2 Bed 4) | Every 3 weeks | Apr–Jun |
| Leeks | 3 indoor sowings (Batch 2 missed) | Staggered | Feb–Mar |

No succession needed for: tomatoes, potatoes, garlic, onions, shallots, cauliflower, PSB, chard, strawberries, basil.

---

## Activity Badge Types

| Badge class | Label | Colour | Meaning |
|-------------|-------|--------|---------|
| `.sow` | SOW | Amber | Sow indoors |
| `.transplant` | T/PLANT | Green | Transplant outdoors |
| `.direct` | DIRECT/PLANT | Blue | Direct sow or plant sets |
| `.harvest` | HARVEST | Red | Harvest window |
| `.chit` | CHIT | Purple | Chit seed potatoes |
| `.general` | TASK | Brown | General tasks |

Gantt chart uses single-letter codes: S=Sow, P=Pot On, T=Transplant, D=Direct, H=Harvest, C=Chit.

---

## Key Growing Notes (Domain Knowledge)

- **No-dig**: Never dig beds. 15cm compost over cardboard each season. Do not compact beds.
- **Carrots on clay**: Pierce cardboard with crowbar, fill holes with compost + sharp sand, mound 10cm high.
- **Brassicas exception**: Firm soil before planting (the only compaction exception in no-dig).
- **Clay timing**: Delay direct sowing 1–2 weeks vs. lighter soils. Start indoors where possible.
- **Chitting potatoes**: Egg boxes, rose end up, cool bright room (not warm). 4–6 weeks before planting.
- **Earthing up potatoes**: Use compost + straw as shoots emerge.
- **Tomatoes**: Harden off 7–10 days before transplanting. Feed weekly once first truss appears.
- **Spinach**: Never sow in July (bolts in heat). Restart late August.
- **Nasturtiums**: Essential trap crop — aphids prefer them over vegetables. Every bed corner.
- **Leeks**: Transplant into dibber holes through cardboard (no need to fill hole — just water in).
- **Garlic space re-use**: Transplant leeks into garlic/onion gaps as they are harvested in July.

---

## PWA / Service Worker

The service worker (`sw.js`) uses a cache-first strategy:
1. On install: caches `./`, `./index.html`, `./manifest.json`, `./icon-192.png`, `./icon-512.png`
2. On activate: deletes any old caches with a different name than `allotment-2026-v1`
3. On fetch: returns cached response if available; otherwise fetches from network and caches the response; falls back to `./index.html` on network failure

To bust the cache after updates, increment the version string in `sw.js`:
```javascript
const CACHE_NAME = 'allotment-2026-v1'; // change v1 to v2 etc.
```

---

## Deployment

GitHub Pages serves directly from the `main` branch root. No build step required.

1. Push changes to `main`
2. GitHub Pages automatically serves the updated files
3. The `.nojekyll` file prevents Jekyll processing (required for the service worker to work correctly)

---

## Editing Guidelines

- **All content is hardcoded HTML** — there is no data layer, no JSON data file, no templating.
- When adding a crop task to the calendar, add it to all relevant sections: the monthly card (`#calendar`), the Gantt table rows, the Gantt card view weekly items, the bed layout (`#beds`), the succession table (`#succession`), the progress tracker (`#progress`), the growing guide (`#guide`), and potentially the advice section (`#advice`).
- The `#progress` section contains a "Completed Sowings" table and "Upcoming Actions" table — update these to reflect actual vs planned dates.
- The `#guide` section uses `<details class="checklist-details">` expandable cards with `<input type="checkbox">` items — one card per crop.
- The `#checklist` section is a week-by-week interactive checkbox list for windowsill sowing tasks.
- Month card header colours use classes like `.mc-feb`, `.mc-mar`, etc. — defined in CSS.
- Bed highlight row colours use classes like `.hl-garlic`, `.hl-onion`, `.hl-carrot`, `.hl-straw`, `.hl-brass`, `.hl-potato`, `.hl-tomato`, `.hl-leafy` — defined in CSS.
- Bed card headers in `#beds` use `.bed1-header` (green), `.bed2-header` (brown), `.bed3-header` (purple), `.bed4-header` (red) classes on the `.card-header` div.
- The Gantt table has 39 week columns (Feb week 2 through Oct week 26). Each row must have exactly 39 `<td class="gantt-cell">` cells after the 3 fixed columns.
- Gantt month header row (`.gantt-months th`) is sticky at `top:0`. Week header row (`.gantt-week-cell`) is sticky at `top:35px`.
- Themes are applied via `data-theme=""` (garden/default), `data-theme="slate"`, or `data-theme="terracotta"` on `<html>`. Three coloured dot buttons live in the hero section.
- `localStorage` keys: `allotment-theme` (theme name), `allotment-checks` (JSON map of checkbox id → boolean).
- Typography: headings use `Playfair Display` (serif), body uses `Source Sans 3` (sans-serif), both loaded from Google Fonts.

---

## Season

This calendar covers the **2026 growing season** (February 2026 – October 2026, with PSB overwintering to spring 2027). The year is hardcoded in the title, manifest, and service worker cache name.
