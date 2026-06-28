# Subway Builder Custom Maps — Greater Tokyo (TYO)

Custom map packs for [Subway Builder](https://store.steampowered.com/app/2659390/Subway_Builder/) — a transit simulation game by Anomaly. This repo hosts release builds of the **Greater Tokyo** map pack, covering the Kantō region of Japan.

## Overview

The Greater Tokyo map pack models the world's largest metropolitan area — home to over 24 million residents across Tokyo, Kanagawa, Saitama and Chiba. The pack uses real-world census data, OpenStreetMap road networks, MLIT infrastructure datasets, and official visitor statistics to create a realistic transit demand simulation.

| Statistic | Value |
|-----------|-------|
| **Population** | 24,014,925 |
| **Commuter population** | 20,106,263 |
| **Special demand population** | 3,908,662 |
| **Demand points** | 30,642 |
| **Special demand points** | 8,041 |
| **Coverage area** | ~10,900 km² (188 municipalities) |
| **PMTiles zoom** | z7–z15 (19,669 tiles) |
| **Ocean depth tiles** | 15,557 tiles with 76,313 bathymetry features |
| **Special demand types** | 6 categories (airports, hospitals, schools, universities) |


## Data Sources

| Data | Source |
|------|--------|
| **Population & jobs** | Japanese national census (e-Stat), economic census, cho-aza (town-block) level data |
| **Road network** | OpenStreetMap (Kantō region extract) |
| **Buildings** | OpenStreetMap building footprints via Protomaps/Planetiler |
| **Base map tiles** | Protomaps Basemap v4, built from OSM with Planetiler |
| **Bathymetry** | GEBCO 2024 ocean depth grid |
| **Airports** | MLIT airport data (国交省航空局) |
| **Hospitals** | MLIT medical institution data |
| **Schools** | MLIT school data, MEXT enrolment statistics |
| **Universities** | NIAD-QE institution data |
| **Ports** | MLIT port data |

## What's New in v0.6.0

### Full Water Coverage

The PMTiles clip polygon now includes water rectangles for Tokyo Bay, the Shonan coast, and Sagami Bay. The map renders blue water throughout the entire bay and coastal areas — not just a thin strip hugging the coastline.

- **19,669 PMTiles** with full z7–z15 zoom range
- **15,557 tiles with ocean depth** — GEBCO 2024 bathymetry injected as ocean foundation polygons (depth −200 m to −1 m)
- **76,313 bathymetry features** across 8 depth bands

### Industrial Port Islands

Artificial port islands (Daikoku, Ogishima, Higashi-Ogishima, Minamihonmoku) have **zero residents** — nobody lives there — but retain **real worker/job counts** based on census employment data. A spatial bbox filter catches demand points snapped onto these islands after OSM place-node snapping, and redistributes their residents to non-industrial cells in the same municipality.

### Demand Allocation

- **Cho-aza census-based allocation** — Population and jobs allocated from town-block level census data using polygon boundary matching (102 municipalities) and Voronoi assignment (91 municipalities)
- **Voronoi uniform fallback** — Municipalities with <10% cho-aza geocode rate get uniform distribution to avoid single-seed population collapse
- **Municipality totals match census data exactly** — no population loss
- **No demand point exceeds 10,000 residents**

### Pipeline Improvements

- O/D flow generation moved after snap + industrial zone filter for correct demand point states
- Post-snap industrial zone filter in snap script
- Display PMTiles builder expanded south/east visual buffers (0.20°) to include all water tiles
- PMTiles Docker build uses clip polygon with `--clip-buffer=0` for exact municipality + water boundary clipping

## Pack Contents

| File | Size | Description |
|------|------|-------------|
| `TYO.pmtiles` | 181 MB | Vector basemap tiles (z7–z15) with ocean foundations |
| `buildings_index.json` | 195 MB | Building footprints quadtree index (936,786 buildings) |
| `demand_data.json` | 56 MB | Demand points and O/D pops |
| `water.geojson` | 10 MB | Water polygon features |
| `roads.geojson` | 24 MB | Road network |
| `ocean_depth_index.json` | 3.8 MB | GEBCO bathymetry depth index |
| `.railyard_map/special_demand_points.json` | 2.6 MB | 8,041 special demand points |
| `runways_taxiways.geojson` | 1.2 MB | Airport runways |
| `thumbnail.svg` | 157 KB | SVG thumbnail |
| `config.json` | 4 KB | Pack metadata |

## Version History

| Version | ZIP size | Pops | Population |
|---------|----------|------|------------|
| **v0.6.0** | **214 MB** | **253,334** | **24,014,925** |
| v0.5.0 | 269 MB | 606,129 | 29,536,566 |

## Changelog

### v0.6.0 (June 2026)
- Feature: Full Tokyo Bay, Shonan coast, and Sagami Bay water coverage in PMTiles
- Feature: GEBCO 2024 bathymetry ocean foundations (15,557 tiles, 76,313 features)
- Feature: Industrial port islands — zero residents, real job counts preserved
- Feature: Cho-aza census-based demand allocation (188 municipalities, exact totals)
- Feature: Voronoi uniform fallback for missing cho-aza boundary data
- Feature: Post-snap industrial zone filter with resident redistribution
- Feature: Full z7–z15 zoom range retained (no zoom reduction)
- Fix: Minamihonmoku false residents (Voronoi collapse fixed by spatial filter)
- Fix: Daikoku/Ogishima false residents (zeroed with redistribution)
- Fix: O/D flow generation now runs after snap + industrial filter

### v0.5.0 (April 2026)
- Improved demand point generation and flow routing

### v0.4.0 (April 2026)
- Pack refinements and data updates

### v0.3.0 (April 2026)
- Expanded special demand coverage and flow improvements

### v0.2.0 (April 2026)
- Full changelog: [v0.1.0...v0.2.0](https://github.com/jelegend/subwaybuilder/compare/v0.1.0...v0.2.0)

### v0.1.0 (April 2026)
- Beta release for custom map of Greater Tokyo area

## License

Apache License 2.0 — see [LICENSE](LICENSE).

Map data © OpenStreetMap contributors (ODbL). Census data from e-Stat (Japan). Bathymetry from GEBCO. Infrastructure data from MLIT.
