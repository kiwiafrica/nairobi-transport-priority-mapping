# Methodology (research summary)

This document describes the methods as reported in the supplied 2026 master's thesis. It is **not** a runnable implementation.

## Study units

The study reconstructed 47 medium zones from JICA's NIUPLAN reference map. Boundaries were georeferenced and digitized for research, with area-based calculations in **WGS 84 / UTM zone 37S (EPSG:32737)**. These are not authoritative boundaries.

## Built-up area

Dynamic World V1 built-class probability was summarized using annual median composites for **2016** and **2023** in Google Earth Engine. A threshold of **0.64** was selected after comparison of candidate thresholds with ESA WorldCover 2021. The resulting built-up areas were aggregated by zone.

## Buildings and other proxies

The research filtered Google Open Buildings to features with confidence **≥ 0.7**, using building counts and a volume proxy (footprint area × height). KNBS population data, grouped OSM POIs, and Matatu stops were aggregated to the zone level. Original extraction scripts and intermediates are not present in this repository.

## Index definitions

For all variables, apply min-max normalization across the study zones:

`norm(x_i) = (x_i - min(x)) / (max(x) - min(x))`

Then:

- `TG = (norm(population) + norm(building_count) + norm(built_up_2023)) / 3`
- `TA = (norm(commercial_office_POIs) + norm(education_health_POIs) + norm(industry_POIs) + norm(building_volume)) / 4`
- `Demand = (TG + TA) / 2`
- `Growth = norm(built_up_2023 - built_up_2016)`
- `CorePriority = (Demand + Growth) / 2`
- `MatatuGap = 1 - norm(matatu_stops / zone_area)`

The Matatu supply-gap variable was **not** added to CorePriority in the primary model; it was used for complementary interpretation and tested in alternative specifications.

## Validation

- Spearman rank correlation of TA index and Matatu stop count
- Repeat correlations after excluding three core business zones
- Rank-based partial correlations controlling for zone area and alternative density-based indices
- Complementary comparison with NIUPLAN's 2030 planning spatial patterns
- PCA-based versus equal weighting sensitivity, and transport supply inclusion scenarios

## Interpretation

The research prioritizes **screening for further study**, not estimating absolute trip volumes or selecting construction projects. Area effects, uneven zone sizes, spatial reporting biases and mismatched data vintages remain limitations.
