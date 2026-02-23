# 🌱 2026 Allotment Planting Calendar

A week-by-week planting calendar for four raised beds (2.4m × 1.2m) on heavy clay soil using no-dig methods.

**Features:** Companion planting · Succession sowing · Trap crops · Offline PWA

## Deploying to GitHub Pages

1. Push all files in this repo to the `main` branch
2. Go to **Settings → Pages**
3. Under **Source**, select **Deploy from a branch**
4. Choose `main` branch and `/ (root)` folder
5. Click **Save**

Your site will be live at `https://<username>.github.io/<repo-name>/`

## Files

| File | Purpose |
|------|---------|
| `index.html` | The full calendar app |
| `manifest.json` | PWA manifest (app name, icons, theme) |
| `sw.js` | Service worker for offline caching |
| `icon-192.png` | Home screen icon (192×192) |
| `icon-512.png` | Splash screen icon (512×512) |
| `.nojekyll` | Tells GitHub Pages not to process with Jekyll |

## Using as a PWA

Once the page is live, visit it on your phone and use **"Add to Home Screen"** — it will install as a standalone app that works offline.
