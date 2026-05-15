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
| `updates.html` | Password-protected progress update form — see Progress Update Workflow below |
| `manifest.json` | PWA manifest (app name, icons, theme colour, display mode) |
| `sw.js` | Service worker — caches all assets for offline use (cache name: `allotment-2026-v7`) |
| `archive-plan-2026.html` | Read-only archive of the original February 2026 season plan, before the May 2026 re-sync |
| `bed1-layout.html` | Interactive SVG layout for Bed 1 (Alliums, Roots & Leeks) — month-by-month plant positions with zoom, tooltips, and planting guide |
| `bed2-layout.html` | Interactive SVG layout for Bed 2 (Potatoes, Beetroot & Roots) — shows earthing-up ridges growing over the season |
| `bed3-layout.html` | Interactive SVG layout for Bed 3 (Brassicas & Shallots) — shows shallot chequerboard, cauliflower and PSB positions with netting indicators |
| `bed4-layout.html` | Interactive SVG layout for Bed 4 (Tomatoes, Leafy Greens & Herbs) — shows tomato shading footprint and succession lettuce rows |
| `icon-192.png` | Home screen icon (192x192) |
| `icon-512.png` | Splash screen icon (512x512) |
| `gantt-data.md` | Raw Gantt data (all crops, dates, activities) for import into MS Project or similar |
| `.nojekyll` | Prevents GitHub Pages from processing with Jekyll |

Everything lives in `index.html`. Do not split it into separate files unless explicitly asked.

---

## Application Structure (index.html)

### Sections (Tab Navigation)

The app has seven tabs, each a `<div id="..." class="section">`. Only the active section has the `.active` class (shown via CSS `display:block`).

| Tab ID | Label | Content |
|--------|-------|---------|
| `calendar` | Month-by-Month | Monthly task cards from Feb–Oct |
| `progress` | 📊 Progress Tracker | Actual vs planned progress table, windowsill occupancy, upcoming actions |
| `gantt` | Week-by-Week Gantt | Placeholder pointing to `gantt-data.md` (chart removed 07 May 2026) |
| `beds` | Bed Layouts | Visual diagrams of all 4 beds |
| `succession` | Succession Detail | Full table of every sowing/transplanting date |
| `checklist` | Weekly Checklist | Week-by-week interactive checkbox sowing guide |
| `guide` | 🌿 Growing Guide | Crop-by-crop expandable instructions (seed to harvest) |
| `advice` | Growing Advice | Advice cards on techniques |

### JavaScript Functions

```javascript
// Switch between main navigation tabs
function showSection(id, btn) { ... }

// Set colour theme ('garden' | 'slate' | 'terracotta') and persist to localStorage
function setTheme(name, btn) { ... }
```

Two self-invoking init functions also run on page load:
- `initTheme()` — restores saved theme from `localStorage`
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

- Mobile (max 700px): Nav tabs compact.
- Desktop (min 701px): Full layout.

---

## Data: Beds and Crops

### Bed 1 — Alliums, Roots & Leeks
Layout north → south (as planted):
- Onion sets (Sturon) — planted 20 Mar, harvest Jul/Aug
- Leeks (Musselburgh) — **3 batches**: Batch 1 sown 14 Feb (standard tray, transplant 28 May), Batch 3 sown 6 Mar (deep root trainer, transplant 8 Jun into garlic gap), Batch 4 sown 6 May (deep root trainer, transplant mid-Jul into onion gap). ~60 leeks total.
- Carrots (Chantenay Royal) — 4 succession sowings May–Jul, direct sow through cardboard
- Marigolds (Mango Tango) — companion between carrots
- Garlic (Germidour) — in ground (south end), harvest Jul
- Nasturtiums — all 4 corners, trap crop

### Bed 2 — Potatoes, Beetroot & Roots
*(was Bed 3 before 07 Mar 2026 renumbering)*
- 1st Early Potatoes (Casablanca) — 18 sets, planted 6 May, harvest mid-Jul
- 2nd Early Potatoes (Charlotte) — 18 sets, planted 6 May, harvest late Jul–Aug
- Maincrop Potatoes (Setanta Organic) — 18 sets, planted 6 May, harvest Sep–Oct
- Spinach (Medania) — between potato rows
- Beetroot (Boldor) — south end, 5 succession sowings May–Jul (first sown 6 May)
- Radish (French Breakfast) — alongside beetroot, sowings May–Jun
- Nasturtiums — alongside potatoes

### Bed 3 — Brassicas & Shallots
*(was Bed 2 before 07 Mar 2026 renumbering)*
- PSB / Purple Sprouting Early — sown 6 May (4 cells), transplant late Jun/Jul, overwinters to spring 2027
- Shallot sets (Biztro) — planted 20 Mar, harvest Jul/Aug
- Cauliflower (All Year Round) — sown 6 May (6 cells), transplant mid-Jun, net immediately
- Nasturtiums (Tom Thumb) — all 4 corners (essential for brassica aphid control)

### Bed 4 — Tomatoes, Leafy Greens & Herbs
- Tomatoes x3: Moneymaker, Black Opal, Gardeners Delight — sown 24 Feb (72 seeds). In 9cm pots (~30 plants). Harden off 19 May; transplant best 7–8 plants 26 May (north end). Gift remaining ~12 plants.
- Basil (Greek) — sown 6 May, transplant early Jun between tomatoes
- Marigolds (Spanish Brocade) — sown 6 May, transplant late May/Jun between tomatoes
- Radish (French Breakfast) — 2 sowings at tomato base May/Jun
- Chard (Rainbow) — sown 6 May (5 cells), transplant early Jun; 2nd direct sow 20 Jul for overwintering
- Lettuce (Little Gem) — south end, **8 direct sowings** every 2 weeks from 6 May through 12 Aug
- Spinach (Medania) — south end, direct sow May, restart late Aug

---

## Succession Sowing Summary

| Crop | # Sowings | Frequency | Season |
|------|-----------|-----------|--------|
| Lettuce | 8 (direct only) | Every 2 weeks | 6 May → 20 May → 3 Jun → 17 Jun → 1 Jul → 15 Jul → 29 Jul → 12 Aug |
| Spinach | ~3 | Every 3 weeks | May, then restart late Aug (AVOID July) |
| Carrots | 4 | Every 3–4 weeks | May–Jul |
| Beetroot | 5 | Every 3 weeks | May–Jul (first sown 6 May) |
| Radish | ~5 (Bed 2 + Bed 4) | Every 3 weeks | May–Jun |
| Leeks | 3 indoor sowings | Staggered | Feb (Batch 1), Mar (Batch 3), May (Batch 4) |

No succession needed for: tomatoes, potatoes, garlic, onions, shallots, cauliflower, PSB, chard, basil.

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

Gantt data (activity codes: SOW/DIRECT/TRANSPLANT/CHIT/HARVEST/TASK) is in `gantt-data.md` — import into MS Project or any Gantt tool.

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
- **Lettuce**: Direct sow outdoors from May. Surface sow (needs light to germinate) — do not cover seed. Station sow 2–3 seeds every 20cm, thin to 1.

---

## PWA / Service Worker

The service worker (`sw.js`) uses a cache-first strategy:
1. On install: caches `./`, `./index.html`, `./manifest.json`, `./icon-192.png`, `./icon-512.png`
2. On activate: deletes any old caches with a different name than the current cache name
3. On fetch: returns cached response if available; otherwise fetches from network and caches the response; falls back to `./index.html` on network failure. `updates.html` is explicitly excluded from caching — always fetched fresh.

To bust the cache after updates, increment the version string in `sw.js`:
```javascript
const CACHE_NAME = 'allotment-2026-v6'; // increment on each deploy
```

The SW uses `skipWaiting()` + `clients.claim()` so a new SW activates immediately on install. The page registration script listens for `controllerchange` and calls `window.location.reload()` — this means the installed PWA **auto-refreshes** on the user's device when new code is deployed, without needing to delete and re-add to the home screen.

**Hero overlay note:** `.hero::before` has `pointer-events:none` — this is required so the decorative stripe pseudo-element does not block clicks on the theme picker buttons inside the hero.

---

## Deployment

GitHub Pages serves directly from the `main` branch root. No build step required.

1. Push changes to `main`
2. GitHub Pages automatically serves the updated files
3. The `.nojekyll` file prevents Jekyll processing (required for the service worker to work correctly)

---

## Editing Guidelines

- **All content is hardcoded HTML** — there is no data layer, no JSON data file, no templating.
- When adding a crop task to the calendar, add it to all relevant sections: the monthly card (`#calendar`), the bed layout (`#beds`), the succession table (`#succession`), the progress tracker (`#progress`), the growing guide (`#guide`), and potentially the advice section (`#advice`). Also update `gantt-data.md`.
- The **bed layout files** (`bed1-layout.html` through `bed4-layout.html`) are standalone interactive SVG diagrams. They each have their own hardcoded crop positions in JavaScript constants at the top of their `<script>`. If plant positions, varieties, or dates change materially, update the relevant bed layout file's JS constants too. Each file has a `← Back to planner` link to `index.html`.
- The `#progress` section contains a "Completed Sowings" table and "Upcoming Actions" table — update these to reflect actual vs planned dates.
- The `#guide` section uses `<details class="checklist-details">` expandable cards with `<input type="checkbox">` items — one card per crop.
- The `#checklist` section is a week-by-week interactive checkbox list for windowsill sowing tasks.
- Month card header colours use classes like `.mc-feb`, `.mc-mar`, etc. — defined in CSS.
- Bed highlight row colours use classes like `.hl-garlic`, `.hl-onion`, `.hl-carrot`, `.hl-straw`, `.hl-brass`, `.hl-potato`, `.hl-tomato`, `.hl-leafy` — defined in CSS.
- Bed card headers in `#beds` use `.bed1-header` (green), `.bed2-header` (brown), `.bed3-header` (purple), `.bed4-header` (red) classes on the `.card-header` div.
- Themes are applied via `data-theme=""` (garden/default), `data-theme="slate"`, or `data-theme="terracotta"` on `<html>`. Three coloured dot buttons live in the hero section.
- `localStorage` keys: `allotment-theme` (theme name), `allotment-checks` (JSON map of checkbox id → boolean).
- Typography: headings use `Playfair Display` (serif), body uses `Source Sans 3` (sans-serif), both loaded from Google Fonts.

---

## Season

This calendar covers the **2026 growing season** (February 2026 – October 2026, with PSB overwintering to spring 2027). The year is hardcoded in the title, manifest, and service worker cache name.

---

## Progress Update Workflow

The owner uses `updates.html` to record task progress and generate a structured prompt for Claude to process. This is the canonical way to keep the site, Asana, and CLAUDE.md in sync.

### How it works
1. Owner visits `https://davideuantrott.github.io/allotmentplanner/updates.html`
2. Logs in with password (SHA-256 protected; hash stored in `updates.html`)
3. Sets status, actual date, and notes for any changed tasks
4. Clicks **Generate Claude Prompt** → copies the output block
5. Pastes into a new Claude Code chat

### What Claude does when receiving a prompt from updates.html
1. Read `CLAUDE.md` for full project context and current season status
2. Parse each task update and determine what changed vs the plan
3. Update `index.html` across all relevant sections: progress tracker (completed sowings, windowsill occupancy, upcoming actions), calendar month cards, succession table, checklist. Also update `gantt-data.md` if dates shift. Update the "Last Updated" date.
4. Sync Asana (project GID: `1213549975842888`, user GID: `1208097174845383`): mark tasks complete/incomplete, update due dates, add a story comment, update task description if the plan changed. For tasks with GID prefixed `new_`, search by name first and create if not found.
5. Update `CLAUDE.md` season status section
6. Commit and push with a descriptive message

### Keeping updates.html in sync with the plan
When task dates or names change in `index.html`, update the corresponding entry in the `TASKS` array in `updates.html`. Each entry has: `gid` (Asana task GID), `name`, `due` (ISO date), `month`, `status`, and optional `note` (pre-filled context).

### Access
- Direct URL: `https://davideuantrott.github.io/allotmentplanner/updates.html`
- Also reachable via the faint ⚙ icon fixed to the bottom-right of the main page
- Password is set and active as of 20 March 2026
- `updates.html` is never cached by the service worker — always served fresh

---

## Season Status (as of 6 May 2026)

> **Note:** The site was re-planned on 6 May 2026 as a fresh forward-looking plan. All sections of index.html show what IS growing and what the plan is from here — no failure/missed language anywhere on the site. CLAUDE.md retains technical accuracy for context but the site philosophy is positive and forward-looking.

### Established & Growing
- ✅ Leeks Batch 1 — sown 14 Feb (standard tray, 12 cells). Transplant 28 May to Bed 1.
- ✅ Leeks Batch 3 — sown 6 Mar (deep root trainer, 12 cells). Transplant 8 Jun to Bed 1 (garlic gap).
- ✅ Tomatoes ×3 — sown 24 Feb (72 seeds). Potted on 29 Apr (~30 plants in 9cm pots). Feeding weekly. Harden off 19 May; transplant best 7–8 plants 26 May.
- ✅ Onion sets (Sturon) — planted 20 Mar, Bed 1. Growing.
- ✅ Shallot sets (Biztro) — planted 20 Mar, Bed 3. Growing.
- ✅ Garlic (Germidour) — in ground since autumn, Bed 1 south end. Harvest Jul.
- ✅ Potatoes Casablanca (18 sets), Charlotte (18 sets), Setanta (18 sets) — all chitted. Plant 6 May, Bed 2.

### Sown 6 May 2026 (the new plan's starting point)
- Leeks Batch 4 (26 seeds, deep root trainer) — transplant mid-July
- Cauliflower (12 seeds, 6 cells) — transplant mid-June
- PSB Batch 1 (8 seeds, 4 cells) — transplant late June/Jul
- Chard Batch 1 (10 seeds, 5 cells) — transplant early June
- Companion plants: nasturtiums direct, marigolds indoor, basil warmest spot
- Lettuce #1 direct sow (Bed 4 south)
- Beetroot Batch 1 direct sow (Bed 2 south)
- Radish Batch 1 direct sow (Bed 2 alongside beetroot)

### Lettuce Plan (direct-sow only, from 6 May)
All indoor attempts discarded/abandoned. Plan is 8 direct sowings every 2 weeks:
6 May → 20 May → 3 Jun → 17 Jun → 1 Jul → 15 Jul → 29 Jul → 12 Aug

### Upcoming
- 19 May: Begin hardening off tomatoes
- 20 May: Lettuce direct sow #2
- 26 May: Transplant tomatoes to Bed 4 (best 7–8 plants)
- 28 May: Transplant Leeks Batch 1 to Bed 1
- 8 Jun: Transplant Leeks Batch 3 to Bed 1 (garlic gap)
- Mid-Jun: Transplant cauliflower to Bed 3
- Late Jun/Jul: Transplant PSB to Bed 3; Transplant Leeks Batch 4 to Bed 1 (onion gap)
