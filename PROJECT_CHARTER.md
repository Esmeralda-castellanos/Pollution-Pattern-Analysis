# Project Charter — Pollution Pattern Analysis

## SMART Clustering Objective
- **Who:** RIVM Luchtmeetnet air-quality monitoring stations across the Netherlands
- **What:** Annual multi-pollutant profile clusters (NO2, PM2.5, PM10, O3), 2024 validated data
- **Why:** Help environmental agencies, municipalities, and public-health organisations move from
  one-size-fits-all air-quality management to profile-aware, targeted monitoring and mitigation
- **When:** Delivered across this capstone's milestone schedule, ending in an impact-reporting
  section in the final notebook

## Anticipated Clustering Family
**Primary approach: Partitional clustering (K-Means).**
Rationale:
- The primary features are continuous numerical annual summary statistics per pollutant (mean,
  median, std, 90th percentile, etc.) — the classic case where K-Means performs well.
- The dataset size (one row per station, likely tens to low hundreds of stations) is well within
  K-Means' efficient range, and centroids are directly interpretable as "typical profiles."
- No strong prior expectation of extreme non-spherical cluster shapes in the pollutant-profile
  feature space.

**Secondary comparison: Hierarchical clustering (agglomerative, Ward linkage).**
Used as a validation check against K-Means, since it doesn't require pre-specifying k and can
reveal nested structure (e.g. a "high pollution" branch splitting further into traffic- vs
particulate-dominated sub-groups).

**Considered and set aside for this stage:**
- *DBSCAN* — station density/coverage across the Netherlands is uneven, and DBSCAN struggles when
  cluster sizes and densities differ substantially; it's more natural for the spatial/geographic
  hotspot question than for the multi-pollutant profile question this project targets first.
- *Gaussian Mixture Models* — the soft/probabilistic assignment is appealing (a station could
  plausibly sit between "traffic" and "mixed high-pollution"), but adds complexity not justified
  for a first capstone iteration. Flagged as a possible extension.

## Data Characteristics Summary
- Continuous numerical (dominant): pollutant summary statistics per station
- Spatial (secondary, used for mapping/interpretation only, not as a clustering feature)
- Temporal (used only to construct annual/seasonal summary features, not as raw time series input)