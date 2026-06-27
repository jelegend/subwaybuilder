# Subway Builder Custom Maps — Greater Tokyo (TYO)

Custom map packs for [Subway Builder](https://store.steampowered.com/app/2659390/Subway_Builder/) — a transit simulation game by Anomaly. This repo hosts release builds of the **Greater Tokyo** map pack, covering the Kantō region of Japan.

## Overview

The Greater Tokyo map pack models the world's largest metropolitan area — home to over 28 million residents across Tokyo, Kanagawa, Saitama, Chiba, and parts of Ibaraki, Tochigi, and Gunma. The pack uses real-world census data, OpenStreetMap road networks, MLIT infrastructure datasets, and official visitor statistics to create a realistic transit demand simulation.

| Statistic | Value |
|-----------|-------|
| **Population** | 28,239,444 |
| **Commuter population** | 21,160,835 |
| **Special demand population** | 7,078,609 |
| **Demand points** | 42,766 |
| **Special demand points** | 19,730 |
| **Coverage area** | ~10,000 km² (50km radius from central Tokyo) |
| **Special demand types** | 19 categories (airports, hospitals, schools, universities, temples, museums, parks, and more) |

## Installation

### Prerequisites

- [Subway Builder](https://store.steampowered.com/app/2659390/Subway_Builder/) installed on Steam
- The game's **mapLoader mod** (automatically detected if installed)

### Steps

1. Download `TYO.zip` from the [latest release](https://github.com/jelegend/subwaybuilder/releases/latest)
2. If you have the mapLoader mod installed, the game can install the pack automatically — just place the ZIP where the mod expects it, or use the in-game mod UI
3. For manual installation:
   - Extract the ZIP contents to your game's city data directory:
     ```
     %APPDATA%\metro-maker4\cities\data\TYO\
     ```
   - Copy the `.pmtiles` file to:
     ```
     %APPDATA%\railyard\tiles\TYO.pmtiles
     ```
   - Copy `thumbnail.svg` to:
     ```
     %APPDATA%\metro-maker4\public\data\city-maps\TYO.svg
     ```
4. Launch Subway Builder and select **Greater Tokyo** from the city selection screen

### Save Compatibility

Saves created with earlier versions (v0.1.0–v0.5.0) are compatible with v0.6.0. The game gracefully handles demand points/pops that were removed in the optimization — their commute history is discarded but the save loads without errors. All stations, tracks, routes, and trains are preserved.

## Data Sources

| Data | Source |
|------|--------|
| **Population & jobs** | Japanese national census (e-Stat), economic census, cho-aza (town-block) level data |
| **Road network** | OpenStreetMap (Kantō region extract, March 2026) |
| **Buildings** | OpenStreetMap building footprints via Protomaps/Planetiler |
| **Base map tiles** | Protomaps Basemap v4, built from OSM with Planetiler |
| **Bathymetry** | GEBCO 2024 ocean depth grid |
| **Airports** | MLIT airport data (国交省航空局) |
| **Hospitals** | MLIT medical institution data |
| **Schools** | MLIT school data, MEXT enrolment statistics |
| **Universities** | NIAD-QE institution data |
| **Ports** | MLIT port data |
| **Attractions** | Official visitor statistics for 95+ major Tokyo attractions |

## What's New in v0.6.0

### Performance Optimization

v0.6.0 introduces major performance optimizations to address the 4GB Electron memory limit that caused stuttering and crashes with large transit networks:

- **Geographic edge pruning** — Removed sparse peripheral areas beyond 50km from central Tokyo, eliminating 13.5% of demand points while retaining 96% of the population
- **Pop consolidation** — Merged 260,606 tiny population units (≤20 people) into nearby larger clusters, reducing total pops from 606,129 to 281,139 (53.6% reduction) with zero population loss
- **Demand point consolidation** — Merged co-located demand points within 200m radius
- **Driving path optimization** — Stripped pre-computed driving paths from small population units (only used for optional visualization, not simulation)

### Other Improvements

- **Special demand schema v4** — 19 type codes with bilingual (Japanese/English) labels, MLIT-sourced infrastructure data
- **Cho-aza census-based demand allocation** — Population and jobs allocated from town-block (cho-aza) level census data to demand points using polygon boundary matching and Voronoi assignment
- **Ocean depth index** — GEBCO 2024 bathymetry with 3,996 depth polygons in 8 depth bands
- **Ocean foundations PMTiles layer** — 5,507 tiles with 77,582 underwater foundation features
- **Custom attractions** — 95 major Tokyo attractions with real annual visitor counts from official sources
- **Proper SVG thumbnail** — Rendered from water polygon data (replaces placeholder)
- **Improved name handling** — All 19,730 special demand points have clean Japanese names

### Size Comparison

| Version | ZIP Size | Pops | Population |
|---------|----------|------|------------|
| v0.1.0 | 230 MB | 229,673 | 229,673 |
| v0.5.0 | 269 MB | 606,129 | 29,536,566 |
| **v0.6.0** | **245 MB** | **281,139** | **28,239,444** |

## Changelog

### v0.6.0 (June 2026)
- Performance: Geographic edge pruning (50km cutoff)
- Performance: Pop consolidation (606K → 281K pops, 53.6% reduction)
- Performance: Demand point consolidation (51K → 43K points)
- Performance: Driving path stripping for small pops
- Feature: Special demand schema v4 with 19 type codes
- Feature: Cho-aza census-based demand allocation (187 municipalities, exact totals)
- Feature: Ocean depth index (GEBCO 2024, 3,996 polygons)
- Feature: Ocean foundations PMTiles layer (5,507 tiles)
- Feature: Custom attractions with real visitor data (95 attractions)
- Feature: Proper SVG thumbnail from water polygons
- Fix: Name format — all special demand points now have clean Japanese names
- Fix: minFlowSize set to 0 (aligns with ITM pack, improves driving path coverage)

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
