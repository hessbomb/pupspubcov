# Pups Pub Covington — Tail Chase List

Site-selection dashboard for a Pups Pub location in Covington, Kentucky. Every commercial
parcel in the city was screened, scored and ranked by an engine optimized to identify prospective Dog Bar locations;
the dashboard presents the standings, the anatomy of each score, a parcel map, and a rating workflow for board review.

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
