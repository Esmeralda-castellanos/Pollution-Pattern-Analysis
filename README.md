# Pollution Pattern Analysis: Identifying Air-Pollution Hotspots and Potential Source Profiles in the Netherlands

An unsupervised machine learning project that clusters Dutch national air-quality monitoring stations
into distinct multi-pollutant exposure profiles, using 2024 annual data from RIVM Luchtmeetnet.

**Capstone challenge:** Clustering for sustainable and equitable decisions (SDG-aligned)
**Author:** Esmeralda Castellanos

---

## Why This Project Exists

National or provincial air-quality averages smooth over real differences between locations. Two
stations can report similar-looking single-pollutant numbers while facing fundamentally different
pollution *profiles* — one traffic-dominated, one clean background, one a mixed high-pollution
outlier. Communities near these stations experience different health burdens even when a national
average would treat them the same.

This project asks: **do Dutch monitoring stations naturally sort into interpretable multi-pollutant
profiles, and which stations fall into which?** Answering it supports more targeted, equitable
decisions than a one-size-fits-all approach — for environmental agencies deciding where to expand
monitoring, municipalities prioritizing interventions, public-health organizations (GGD) assessing
exposure, urban planners siting sensitive land uses, and community/environmental-justice groups.

---

## Key Findings

Clustering 50 monitoring stations (that met a strict data-coverage threshold) on their annual
PM2.5, PM10, and NO2 statistics surfaced **four distinct pollution profiles**:

| Profile | Stations | Confidence | Distinguishing Signature |
|---|---|---|---|
| **Traffic-Influenced, Lower-Particulate** | 18 | High | Only profile with elevated NO2; concentrated in the Randstad urban corridor |
| **PM2.5-Dominant, Traffic-Decoupled** | 12 | Medium | Elevated fine particulates *without* a traffic signal — an unanticipated pattern |
| **Low-Pollution Background** | 9 | Medium | Lowest levels on all pollutants; scattered nationwide, not regional |
| **Elevated Particulate Outliers** | 11 | Low | Highest particulate levels overall, but the least internally consistent group — read as individual stations worth investigating, not one coherent hotspot region |

Three of the four profiles matched the original hypothesis (traffic, background, mixed-high). The
PM2.5-dominant, traffic-decoupled profile was not anticipated and is the most novel finding.

---

## Dataset

- **Source:** [RIVM Luchtmeetnet](https://data.rivm.nl/data/luchtmeetnet) (Dutch National Air Quality
  Monitoring Network), 2024 validated annual data.
- **Raw scope:** 4 pollutant files (PM2.5, PM10, NO2, O3) covering 515 total monitoring locations,
  plus station/component/measurement-series metadata.
- **Coverage challenge:** pollutant coverage across the network is uneven (PM2.5: 66 stations, PM10:
  86, NO2: 87, O3: 50), and only 26 stations report all four pollutants — too few for a reliable
  cluster solution.
- **Final analysis sample:** 50 stations meeting a ≥75% annual hourly-coverage threshold on all three
  primary pollutants (PM2.5, PM10, NO2). O3 was excluded from clustering due to limited coverage and
  used only as a post-hoc descriptive check.

---

## Methodology

**Data cleaning**
- Small-magnitude negative readings (instrument noise near the detection limit) were clipped to zero
  rather than dropped, to avoid biasing station summaries upward.
- A sustained sensor-fault episode at one station (PM10 readings >1,000 µg/m³ for over a day) was
  identified and excluded network-wide via a domain-implausibility threshold (>500 µg/m³), applied
  consistently to all stations rather than only the affected one.

**Feature engineering**
- Annual mean, median, standard deviation, and 90th-percentile statistics computed per pollutant per
  station (12 candidate features), reduced to 10 after dropping features redundant with others
  (correlation > 0.95).
- Features log-transformed where skewed, then standardized (`StandardScaler`) before clustering.
- Latitude/longitude deliberately withheld from clustering features (used only for post-hoc mapping),
  so clusters reflect pollutant chemistry, not raw geography.

**Cluster-tendency check**
- Hopkins statistic (0.57–0.68) indicated weak-to-moderate genuine cluster structure — set expectations
  for a soft, not sharply separated, solution before any algorithm was run.

**Algorithm comparison**
- **K-Means** (primary): tested k = 2–8; k = 4 selected via silhouette score, Davies-Bouldin index, and
  balanced cluster sizes (18 / 12 / 9 / 11).
- **DBSCAN**: no parameter setting produced a stable, usable clustering — reported as a genuine finding
  consistent with the weak density contrast in the data, not a failed experiment.
- **Agglomerative Hierarchical (Ward linkage)**: used as a secondary validation check; its 3–4 branch
  structure broadly agreed with the K-Means solution.

**Validation & honesty checks**
- Permutation test (1,000 shuffles): observed clustering beat every random permutation (p < 0.0001) —
  the structure is real, not noise.
- Bootstrap stability (Adjusted Rand Index across 20 resamples): mean ARI = 0.431 — **low stability**,
  meaning cluster boundaries shift meaningfully depending on which stations are sampled. Reported as a
  headline limitation, not hidden.
- Per-cluster silhouette breakdown and geographic spatial-coherence scores were computed to identify
  which specific clusters (0, and to a lesser extent 2) are well-formed versus which (3) are loosely
  defined.
- Cohen's d effect sizes confirmed every cluster is statistically distinct on its top features, even
  where internal cohesion is weak — resolving the apparent tension between "statistically real" and
  "loosely grouped" for Cluster 3.

---

## Limitations

- **Sample size and stability:** 50 stations is a small sample for clustering; bootstrap resampling
  showed meaningful instability (mean ARI 0.431), especially for stations near a cluster boundary.
- **Correlation, not causation:** clusters describe pollutant *association*, not confirmed sources.
  Confirming an actual source (traffic, industry, regional transport) would require additional
  evidence (land use, traffic counts, meteorology) outside this project's scope.
- **Coverage bias:** the 50-station sample reflects where reliable monitoring exists, not necessarily
  where pollution is worst — a relevant caveat for environmental-justice applications.
- **Cluster 3 (Elevated Particulate Outliers)** is statistically real (large effect sizes) but
  structurally loose (lowest silhouette, lowest spatial coherence) — station-level conclusions from
  it should be treated as provisional pending individual review.

---

## Repository Structure

```
clustering-project-esmeralda/
├── notebooks/
│   └── clustering_capstone.ipynb   # Full analysis: EDA, cleaning, feature engineering,
│                                    # algorithm comparison, validation, interpretation,
│                                    # and stakeholder impact report
├── data/                            # RIVM Luchtmeetnet source files (not tracked in repo)
├── outputs/                         # Generated figures (station map, cluster comparisons)
└── README.md
```

---

## How to Run

1. Download the 2024 RIVM Luchtmeetnet data files from
   [data.rivm.nl/data/luchtmeetnet](https://data.rivm.nl/data/luchtmeetnet) into `data/`.
2. Install dependencies (pandas, numpy, scikit-learn, matplotlib, scipy).
3. Run `notebooks/clustering_capstone.ipynb` top to bottom.

---

## Tech Stack

Python · pandas · scikit-learn (K-Means, Agglomerative Clustering, DBSCAN, PCA) · matplotlib · scipy

---

## License

Add a license of your choice (e.g. MIT) if you intend this repository to be reused.
