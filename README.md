# Wildfire Datasets Review

A curated, deep review of datasets, databases, and live data endpoints relevant to **wildfire research, prediction modelling, and trend analysis** at a global scale.

## Purpose

This repository serves three interconnected goals:

1. **Data Discovery** — Catalogue every significant source of wildfire-related data (historical, real-time, satellite, climate, regional), with official descriptions and URLs.
2. **Comparison & Selection** — Provide a structured comparison to help choose the best sources for specific use-cases, particularly for a **global daily wildfire prediction model**.
3. **Research Foundation** — Lay the groundwork for a potential academic paper: *"Wildfire on Earth: Trends, Patterns, and Relevant Factors"*.

---

## Repository Structure

```
wildfire-datasets-review/
│
├── datasets/                        ← Classified catalogue of all sources
│   ├── README.md                    ← Classification system & quick-reference table
│   ├── 01_processed_ml_ready.md     ← Pre-processed, ML-ready datasets
│   ├── 02_satellite_remote_sensing.md ← Satellite / active-fire products
│   ├── 03_live_api_endpoints.md     ← Live APIs & real-time endpoints
│   ├── 04_climate_weather.md        ← Climate & weather supporting data
│   ├── 05_regional_national.md      ← Regional / national fire databases
│   └── 06_burned_area_emissions.md  ← Burned-area & emissions databases
│
├── top_sources/                     ← Deep-dive analysis for the top 5 sources
│   ├── README.md                    ← Selection rationale & summary
│   ├── nasa_firms/
│   ├── copernicus_cds/
│   ├── global_fire_emissions_database/
│   ├── effis/
│   └── mtbs/
│
└── research_notes/                  ← Notes for future paper and modelling work
    ├── paper_outline.md             ← Draft outline: trends, patterns, factors
    └── comparison_matrix.md         ← Side-by-side feature matrix of all sources
```

---

## Quick Navigation

| Goal | Start here |
|------|-----------|
| See all data sources at a glance | [`datasets/README.md`](datasets/README.md) |
| ML-ready datasets for quick experimentation | [`datasets/01_processed_ml_ready.md`](datasets/01_processed_ml_ready.md) |
| Real-time / API data for production pipelines | [`datasets/03_live_api_endpoints.md`](datasets/03_live_api_endpoints.md) |
| Best sources for global daily prediction | [`top_sources/README.md`](top_sources/README.md) |
| Supporting climate/weather features | [`datasets/04_climate_weather.md`](datasets/04_climate_weather.md) |
| Paper outline and research direction | [`research_notes/paper_outline.md`](research_notes/paper_outline.md) |
| Full comparison matrix | [`research_notes/comparison_matrix.md`](research_notes/comparison_matrix.md) |

---

## Classification System

All sources are grouped into six categories:

| # | Category | Description |
|---|----------|-------------|
| 1 | **Processed / ML-Ready** | Curated datasets with extracted features, ready for modelling |
| 2 | **Satellite / Remote Sensing** | Raw or processed satellite-derived fire products |
| 3 | **Live APIs / Endpoints** | Web services delivering real-time or near-real-time fire data |
| 4 | **Climate & Weather** | Environmental drivers: temperature, humidity, wind, drought indices |
| 5 | **Regional / National** | Country or region-specific authoritative fire records |
| 6 | **Burned Area & Emissions** | Post-fire impact: burned area, CO₂, smoke emissions |

---

## Key Use-Case: Daily Global Wildfire Prediction

For a model that predicts wildfire occurrence at the region level on a daily basis, the recommended data stack is:

- **Active fire detections**: [NASA FIRMS](top_sources/nasa_firms/README.md) (VIIRS 375 m, near-real-time)
- **Fire weather / climate drivers**: [Copernicus CDS](top_sources/copernicus_cds/README.md) (ERA5, FWI system)
- **Historical fire occurrence**: [GFED](top_sources/global_fire_emissions_database/README.md) + [MTBS](top_sources/mtbs/README.md) (for North America)
- **European context**: [EFFIS](top_sources/effis/README.md)

See [`top_sources/README.md`](top_sources/README.md) for full rationale.

---

## License

[MIT](LICENSE)

