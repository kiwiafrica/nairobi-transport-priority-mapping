# Nairobi Transport Infrastructure Priority Mapping

**Satellite imagery + open geospatial data | Google Earth Engine · QGIS · Transport planning**

Identifying **priority areas for transport infrastructure investment** in the Nairobi Metropolitan Area when recent zone-level trip survey data are unavailable.

> **Scope:** This is a geospatial research and portfolio repository. The indices represent **relative trip-generation and trip-attraction potential**, not measured trips, traffic forecasts, road-capacity estimates, or an approved investment plan.

![Core Priority Index map of Nairobi's 47 analysis zones](figures/05-core-priority-index.png)

*Figure: Core Priority Index across 47 reconstructed NIUPLAN medium zones. The five highest-ranked zones are 24, 25, 2, 26 and 23. Map excerpted from the original thesis.*

## At a glance

| Study area | Analysis framework | Study period | Key finding |
|---|---|---|---|
| Nairobi Metropolitan Area, Kenya | 47 NIUPLAN-based medium zones | Built-up change: 2016–2023 | Built-up area increased by **19.83%** |

**My work:** Satellite and spatial data analysis using Google Earth Engine (GEE), integration of multiple geospatial datasets, and visualization of spatial analysis results in QGIS.

## The problem

Traditional travel demand modeling relies on observed trip-generation (TG) and trip-attraction (TA) data. In a data-scarce setting such as Nairobi, an alternative is needed for *early-stage screening* of areas worth further transport infrastructure investigation.

This project combines satellite-derived urban expansion indicators, built-environment proxies, points of interest, population, and informal public transport information to compare the **relative potential** of areas within the metropolitan region.

## Data and tools

| Dataset | Role in this research |
|---|---|
| **Dynamic World V1** | Built-up area (2016 and 2023); growth pressure |
| **Google Open Buildings** | Building counts and building-volume proxy |
| **KNBS 2019 population** | Population proxy for trip generation |
| **OpenStreetMap POIs** | Commercial/office, education/health, and industrial attraction proxies |
| **Digital Matatus** | Matatu stop distribution for comparison and supply-gap screening |
| **JICA NIUPLAN** | Reference transport-planning zones and 2030 planning comparison |

**Tools:** Google Earth Engine (satellite/spatial analysis), QGIS (geospatial processing and cartographic visualization). The research used the **EPSG:32737** projected coordinate system.

For dataset-specific caveats, see [Data sources and caveats](docs/DATA_SOURCES.md).

## Analytical workflow

1. **Reconstruct analysis zones.** Re-create 47 NIUPLAN medium-zone boundaries from the published reference map for comparative research, rather than representing them as official GIS boundaries.
2. **Integrate spatial variables.** Aggregate built-up area, buildings, population proxies, POIs and Matatu stops within each zone.
3. **Calculate TG and TA potential.** Min-max normalize variables and combine them with equal weights.
4. **Estimate demand and growth.** Combine TG and TA into a Demand Potential Index; separately quantify growth from the 2016–2023 increase in built-up area.
5. **Rank priority zones.** Combine demand and growth into a Core Priority Index and examine a Matatu supply-gap indicator separately.
6. **Check robustness.** Compare against Matatu stop distribution and NIUPLAN; test alternative weights, area effects, and public transport indicators.

**Index construction** (normalized variables are shown with an asterisk):

```text
TG potential     = mean(population*, building count*, 2023 built-up area*)
TA potential     = mean(commercial/office POIs*, education/health POIs*,
                        industrial POIs*, building-volume proxy*)
Demand potential = (TG potential + TA potential) / 2
Growth pressure  = min-max normalized built-up area increase (2016–2023)
Core priority    = (Demand potential + Growth pressure) / 2
Matatu gap       = 1 - normalized(Matatu stops per unit area)
```

The **Matatu Gap is not part of the main Core Priority Index**. It is used as a complementary public transport supply indicator. Full methodological context is in [Methodology](docs/METHODOLOGY.md).

## Spatial analysis outputs

| Growth pressure | Trip-generation potential |
|---|---|
| ![Growth pressure map](figures/01-growth-pressure.png) | ![Trip-generation potential](figures/02-trip-generation-potential.png) |
| **Trip-attraction potential** | **Demand potential** |
| ![Trip-attraction potential](figures/03-trip-attraction-potential.png) | ![Demand potential map](figures/04-demand-potential.png) |

*Maps are excerpts from the research thesis, not newly calculated datasets or rerun analysis.*

## Main findings

### 1. Urban expansion

Built-up area increased from approximately **250.89 km² in 2016** to **300.64 km² in 2023** across the analysis zones: an increase of about **49.75 km² (+19.83%)**.

### 2. Five priority zones

| Rank | Zone | Core priority | Demand | Growth | Matatu gap | Interpretation |
|---:|---:|---:|---:|---:|---:|---|
| 1 | 24 | 0.722 | 0.445 | 1.000 | 0.988 | Growth-driven |
| 2 | 25 | 0.509 | 0.613 | 0.405 | 0.930 | Demand-driven |
| 3 | 2 | 0.491 | 0.318 | 0.663 | 0.925 | Growth-driven |
| 4 | 26 | 0.387 | 0.586 | 0.189 | 0.756 | Demand-driven |
| 5 | 23 | 0.343 | 0.392 | 0.294 | 0.877 | Demand-driven |

The five priority zones all showed Matatu Gap values above the 47-zone median (**0.720**) in this analysis. The table is also available as [CSV](results/top_five_priority_zones.csv).

### 3. Validation and sensitivity

- TA potential versus Matatu stop count: **Spearman ρ = 0.481**, **p = 0.0006** across all 47 zones.
- After excluding three central business zones: **ρ = 0.473**, **p = 0.0012** across 44 zones.
- Equal-weight versus PCA-weight approach: **ρ = 0.999** in ranking; the top-five zone set was unchanged.
- Results should still be interpreted cautiously: area-normalized rankings differ from total-based rankings, and growth pressure is particularly sensitive to zone size.

## Important limitations

- **Not a traffic prediction.** There are no observed zone-level trip counts for calibration; these are **relative potential indices**, not absolute trips/day.
- **Mixed reference years.** Built-up analysis uses 2016/2023, while population and Matatu inputs include 2019 sources.
- **Reconstructed boundaries.** The 47 zones are research-derived from NIUPLAN map materials, not official GIS polygons.
- **Spatial data biases.** Incomplete POIs, areal population allocation, and unequal zone sizes may affect rankings.
- **MAUP/size effects.** Total-based and density-based priority rankings can diverge. The study explicitly treats this as a limitation.

## Code, reproducibility, and rights

This repository currently contains a **research presentation, figures and documented results**. **The original GEE scripts, QGIS project, and source datasets have not yet been provided for publication**, so this repository should **not** be treated as an executable, fully reproducible pipeline.

The original research and maps are based on the author's master's thesis (2026), *Identifying Priority Zones for Transport Infrastructure Investment in the Nairobi Metropolitan Area Based on Trip Generation and Attraction Potential*. Individual datasets retain their respective provider terms. The repository does not grant a new license to data, external basemaps, or thesis material. Please verify publication and attribution rights before making the repository public.

See [Publication checklist](docs/PUBLICATION_CHECKLIST.md) for what to add and verify before release.
