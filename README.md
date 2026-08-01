# Pups Pub Covington — Tail Chase List

Site-selection dashboard for a Pups Pub location in Covington, Kentucky. Every commercial
parcel in the city was screened, scored and ranked; the dashboard presents the standings,
the anatomy of each score, a parcel map, and a rating workflow for field review.

**Live site:** `https://<your-username>.github.io/<repo-name>/`

---

## Publishing this to GitHub Pages

1. Create a **public** repository (GitHub Pages is free only on public repos).
2. Upload every file in this folder, keeping the structure below. Drag-and-drop into the
   GitHub web UI works — no git client required.
3. Go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Set branch to **main** and folder to **/ (root)**. Save.
6. Wait 1–2 minutes. The URL appears at the top of that same Settings → Pages screen.

### File structure — keep this exactly

```
/
├── index.html                              ← the dashboard (must be named index.html)
├── .nojekyll                               ← leave this here, see note below
├── README.md
├── Pups_Pub_Covington_Tail_Chase_List.xlsx ← linked from the dashboard footer
└── data/
    ├── tail_chase_list.json                ← 328 scored parcels
    └── basemap_geometry.json               ← rivers, interstates, arterials
```

`.nojekyll` is an empty file that tells GitHub to serve the folder as-is instead of running it
through Jekyll. Without it, GitHub can silently ignore files and folders beginning with an
underscore. It costs nothing to keep and prevents a class of confusing 404s.

---

## How the dashboard behaves once hosted

- **No build step and no server.** `index.html` is fully self-contained — the parcel data,
  the basemap geometry and the logo are all embedded. It makes no `fetch()` calls.
- **Two external dependencies at render time,** both HTTPS and both optional-degrading:
  - Google Fonts — falls back to system fonts if unreachable
  - LINK-GIS aerial imagery for parcel thumbnails — falls back to a "click for Street View"
    tile if unreachable
- **Ratings save to the visitor's own browser.** Storage is per-browser and per-device, so
  each person who opens the link keeps a private set of ratings. Nothing is shared between
  visitors and nothing is sent anywhere.
- **To collect someone's feedback,** ask them to click **Export ratings** and send you the
  JSON. Use **Import ratings** to merge it into your own copy. Import merges rather than
  overwrites, so several reviewers can be combined.

> If shared, live ratings become a requirement, this needs a small backend
> (a serverless function plus a key-value store). That is a build, not a settings change.

---

## Updating the data later

`index.html` has the parcel data embedded, so replacing `data/tail_chase_list.json` alone will
**not** change what the dashboard shows. Regenerate `index.html` when the model changes, then
upload the new file. The JSON in `data/` is published for reuse and reference, not consumed by
the page.

---

## Data dictionary — `data/tail_chase_list.json`

One object per scored parcel, sorted by chase score descending.

| Key | Meaning |
|---|---|
| `a` | Site address |
| `p` | Parcel ID (PIDN) — the stable key; ratings are stored against this |
| `o` | Owner of record |
| `use` | PVA land-use description |
| `ac` | Lot size, acres |
| `b` | Building size, sq ft — LoopNet gross where available, otherwise GIS footprint |
| `y` | Usable yard, sq ft — open area after a 10 ft morphological erosion |
| `mi` | Miles to the nearest district anchor |
| `nh` | Neighborhood (approximated from coordinates) |
| `ci` | Neighborhood crime index, 0–100, higher is safer |
| `cv` | Interior convertibility class |
| `av` | Availability class |
| `fit` | Fit subtotal out of 100 |
| `s` | **Chase score** = fit × availability multiplier × lot-size multiplier |
| `lat`, `lon` | Parcel centroid, WGS84 |
| `bud` | Budget flag against an $11,000/month target |
| `src` | `LoopNet` (verified building data) or `GIS` (derived footprint) |
| `f` | Factor points, in order: building fit, convertibility, usable yard, location, crime, flood, environmental |

---

## Sources

- **Kenton County PVA / LINK-GIS** — parcels, ownership, building footprints, aerial imagery,
  hydrography, road classification
- **LoopNet** — building gross area and flood zone, matched by APN
- **NeighborhoodScout** — neighborhood safety ranking (5 of 14 neighborhoods; the rest are
  estimated and flagged as such in the workbook)
- **Google Places** — district anchor coordinates

## Known limitations

- **Rent figures are assumed, not quoted** — $11/SF gross. No public Covington lease comps
  exist. Every figure needs broker confirmation.
- **Crime is partly estimated** — only 5 of 14 neighborhoods carry a sourced ranking.
- **Zoning is not scored** — Neighborhood Development Code review sits outside this model, as
  does the Kentucky food-code rule limiting dogs to outdoor dining areas.
- **Building size is mixed-source** — GIS footprints understate multi-storey buildings.
- Parcel ownership and mailing addresses are public record from the Kenton County PVA.
