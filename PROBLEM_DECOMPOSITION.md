# Problem Decomposition — Pollution Pattern Analysis

## Ultimate Goal
Support more equitable, targeted air-quality decisions in the Netherlands by revealing pollution
exposure profiles that national or single-pollutant averages hide.

## Sub-Problems (5W Framework)

| Sub-problem | Who | What | Where | When | Why |
|---|---|---|---|---|---|
| Multi-pollutant station profiling | Luchtmeetnet stations | Annual NO2/PM2.5/PM10/O3 summary stats | Nationwide | 2024 | Discover profile groupings |
| Hotspot geography | Same stations, mapped | Cluster location patterns | Nationwide, by region | 2024 | Identify where profiles concentrate |
| Seasonal variation | Same stations | Winter vs. summer sub-profiles | Nationwide | 2024 | Check if profiles are stable year-round |
| Cluster robustness | Same stations | Stability across seeds/subsets | N/A | 2024 | Ensure findings aren't an artifact |
| Stakeholder translation | Agencies/municipalities | Turning clusters into recommendations | Nationwide | Post-analysis | Make results actionable |

## Impact / Feasibility Matrix

- **High impact, high feasibility (start here):** Multi-pollutant station profiling via K-Means;
  cluster interpretation and stakeholder translation.
- **High impact, lower feasibility (future work):** Linking clusters to demographic vulnerability
  or traffic/industrial emissions data — valuable but requires data this project doesn't yet have.
- **Lower impact, high feasibility (quick wins):** Seasonal winter/summer sub-profile features as
  an optional enrichment to the main clustering.
- **Lower impact, low feasibility (deprioritized):** Multi-year trend clustering — interesting, but
  out of scope given this project uses a single validated year (2024).

## Selected Clustering Problems for This Capstone
1. Cluster Dutch monitoring stations by 2024 multi-pollutant annual profile (core deliverable).
2. Map resulting clusters geographically to support hotspot interpretation (secondary, descriptive
   — not a clustering input).
3. (Stretch goal, time permitting) Compare with a hierarchical clustering solution for robustness.