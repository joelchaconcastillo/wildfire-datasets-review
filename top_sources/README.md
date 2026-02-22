# Top Sources — Selection Rationale & Summary

This directory contains deep-dive analyses of the **five highest-priority data sources** for building a global daily wildfire prediction model. Each sub-directory contains a thorough technical review including data schema, access patterns, known limitations, and integration guidance.

---

## Selection Criteria

Sources were ranked based on the following weighted criteria:

| Criterion | Weight | Rationale |
|-----------|--------|-----------|
| Global coverage | 25% | Model must operate on any region of Earth |
| Daily or sub-daily temporal resolution | 25% | Required for daily prediction |
| Historical depth (≥10 years) | 15% | Sufficient data for robust ML training |
| Official / peer-reviewed provenance | 15% | Scientific validity |
| API / programmatic access | 10% | Automated pipeline feasibility |
| Variable richness | 10% | Multiple predictive features in one source |

---

## The Top 5 Sources

| Rank | Source | Category | Primary Value |
|------|--------|----------|---------------|
| 🥇 1 | [NASA FIRMS](nasa_firms/README.md) | Active Fire / API | Global near-real-time fire detections; longest satellite fire record |
| 🥈 2 | [Copernicus CDS (ERA5 + FWI)](copernicus_cds/README.md) | Climate / API | Pre-computed FWI system; global hourly weather; 45-year record |
| 🥉 3 | [Global Fire Emissions Database (GFED)](global_fire_emissions_database/README.md) | Burned Area / Emissions | Global burned area + emissions; biome breakdown; 23-year record |
| 4 | [EFFIS](effis/README.md) | Regional / API | Best detailed fire database for Europe + Mediterranean; OGC API |
| 5 | [MTBS](mtbs/README.md) | Burned Area / Severity | 40-year Landsat-based US burn severity; highest-resolution severity maps |

---

## Recommended Data Stack for Global Daily Prediction

```
Target variable:        Active fire occurrence (binary) or Fire Radiative Power
Primary fire labels:    NASA FIRMS VIIRS 375m (NRT) + historical archives
Fire weather features:  Copernicus CDS ERA5 + pre-computed FWI (daily, 0.1°)
Burned area history:    GFED monthly + MCD64A1 for spatial features
Vegetation / land:      MODIS NDVI/EVI (MOD13A2), ESA WorldCover
Topography:             SRTM 90m or Copernicus DEM 30m (static features)
Regional enrichment:    EFFIS (Europe), MTBS (USA), INPE/BDQueimadas (Brazil)
```

### Recommended Training Period
**2012–present** — The period with both VIIRS (from 2012) and ERA5 reanalysis available, providing the best combination of fire labels and climate features. For trend analysis, extend back to 2000 using MODIS.

### Spatial Aggregation Strategy
For a daily regional prediction model:
1. Define regions using a consistent global grid (e.g., 0.1° × 0.1° cells matching ERA5-Land)
2. Aggregate VIIRS active fire detections to grid cells (count, max FRP, any-fire binary)
3. Join with daily FWI, ERA5 weather variables, NDVI (16-day interpolated to daily)
4. Binary label: fire occurred in cell that day = 1 / 0

---

## Data Integration Overview

```
[NASA FIRMS] ─────────────────────── fire labels (daily, 375 m → aggregated)
[Copernicus CDS ERA5] ──────────────── weather features (daily, 0.1°)
[Copernicus CDS FWI] ───────────────── fire danger indices (daily, 0.1°)
[GFED burned area] ──────────────── historical burn frequency (monthly)
[MODIS NDVI MOD13A2] ────────────── vegetation state (16-day)
[Copernicus DEM] ────────────────── elevation, slope, aspect (static)
[ESA WorldCover / MCD12Q1] ─────── land cover class (annual)
        ↓
[Feature Matrix: N_cells × N_timesteps × N_features]
        ↓
[ML Model: LSTM / XGBoost / GNN / ConvLSTM]
        ↓
[Daily Fire Probability Map]
```
