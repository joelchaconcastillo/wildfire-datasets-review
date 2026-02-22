# Wildfire Data Sources — Classification Overview

This document provides a structured classification of all identified wildfire-related data sources. Each category has a dedicated file with individual source entries containing a description, URL, coverage, format, and relevance notes.

---

## Categories

| # | File | Category | # Sources |
|---|------|----------|-----------|
| 1 | [01_processed_ml_ready.md](01_processed_ml_ready.md) | **Processed / ML-Ready Datasets** | 10 |
| 2 | [02_satellite_remote_sensing.md](02_satellite_remote_sensing.md) | **Satellite / Remote Sensing Fire Products** | 8 |
| 3 | [03_live_api_endpoints.md](03_live_api_endpoints.md) | **Live APIs & Real-time Endpoints** | 9 |
| 4 | [04_climate_weather.md](04_climate_weather.md) | **Climate & Weather Data** | 8 |
| 5 | [05_regional_national.md](05_regional_national.md) | **Regional / National Fire Databases** | 10 |
| 6 | [06_burned_area_emissions.md](06_burned_area_emissions.md) | **Burned Area & Emissions Databases** | 6 |

**Total catalogued sources: 51**

---

## Quick-Reference Table

| Source | Category | Spatial | Temporal | Global | API | Free |
|--------|----------|---------|----------|--------|-----|------|
| NASA FIRMS (MODIS + VIIRS) | Satellite / API | 375 m – 1 km | Daily / Near-RT | ✅ | ✅ | ✅ |
| Copernicus CDS (ERA5, FWI) | Climate / API | 0.1°–0.25° | Hourly / Daily | ✅ | ✅ | ✅ |
| Global Fire Emissions Database (GFED) | Burned Area / Emissions | 0.25° | Monthly | ✅ | ❌ | ✅ |
| EFFIS (European Forest Fire Information System) | Regional / API | Sub-national | Daily | ❌ (EU+) | ✅ | ✅ |
| MTBS (Monitoring Trends in Burn Severity) | Processed / Satellite | 30 m | Annual | ❌ (USA) | ❌ | ✅ |
| FPA FOD (Fire-Occurrence Database) | Processed / ML-Ready | Point | 1992–2020 | ❌ (USA) | ❌ | ✅ |
| Global Fire Atlas | Processed | 500 m | Daily | ✅ | ❌ | ✅ |
| UCI Forest Fires Dataset | Processed / ML-Ready | Point | 2000–2003 | ❌ (PT) | ❌ | ✅ |
| Kaggle — US Wildfire 1.88 M Records | Processed / ML-Ready | Point | 1992–2015 | ❌ (USA) | ❌ | ✅ |
| Sentinel Hub / EO Browser | Satellite | 10 m – 1 km | Daily | ✅ | ✅ | Freemium |
| NOAA HRRR-Smoke / AirNow | Climate / Air Quality | 3 km | Hourly | ❌ (USA) | ✅ | ✅ |
| Canadian National Fire DB (CNFDB) | Regional | Polygon | 1959–present | ❌ (CA) | ❌ | ✅ |
| NIFC Wildland Fire Statistics | Regional | National | Annual | ❌ (USA) | ❌ | ✅ |
| Australian AFDRS / AFAC | Regional / API | Sub-national | Daily | ❌ (AU) | ✅ | ✅ |
| Brazilian INPE FireMonitor | Regional / API | Sub-national | Daily | ❌ (BR) | ✅ | ✅ |
| NASA EarthData (Earthdata Search) | Satellite / Portal | Varies | Varies | ✅ | ✅ | ✅ |
| ESA Climate Change Initiative (Fire_cci) | Burned Area | 250 m | Monthly | ✅ | ❌ | ✅ |
| MCD64A1 (MODIS Burned Area) | Burned Area | 500 m | Monthly | ✅ | ❌ | ✅ |
| GWIS (Global Wildfire Information System) | Satellite / API | National | Daily | ✅ | ✅ | ✅ |
| Wildfire Prediction Dataset (Kaggle/Aaba) | Processed / ML-Ready | Tile | 2012–2020 | ❌ (USA) | ❌ | ✅ |

---

## Selection Criteria Explained

When evaluating sources for the **daily global wildfire prediction** use-case, the following criteria are weighted:

1. **Spatial resolution** — Finer resolution (< 1 km) preferred for regional modelling
2. **Temporal resolution** — Daily or sub-daily updates critical for operational prediction
3. **Global coverage** — Essential for a globally-applicable model
4. **Historical depth** — Minimum 10-year record needed for robust training
5. **Variable richness** — Fire weather indices, vegetation, climate covariates
6. **API / programmatic access** — Required for automated data pipelines
7. **Open access / free license** — Preference for open data
8. **Official / peer-reviewed provenance** — Scientifically validated sources only

---

## Category Descriptions

### 1. Processed / ML-Ready Datasets
Datasets that have been pre-processed, cleaned, and structured specifically for machine learning or statistical analysis. Often provided as CSV, GeoJSON, or tabular formats with extracted features. Ideal for rapid prototyping.

### 2. Satellite / Remote Sensing Fire Products
Derived products from satellite imagery (MODIS, VIIRS, Landsat, Sentinel). Include active fire detections, burned area maps, fire radiative power (FRP), and land surface temperature. Require more processing but offer highest spatial and temporal resolution.

### 3. Live APIs & Real-time Endpoints
Web services and REST/OGC APIs that deliver current or near-real-time fire data. Essential for operational forecasting systems and dashboard applications.

### 4. Climate & Weather Data
Environmental covariates that drive fire risk: temperature, relative humidity, wind speed, precipitation, drought indices, and derived products like the Fire Weather Index (FWI). Critical as model features.

### 5. Regional / National Fire Databases
Authoritative fire occurrence databases maintained by national agencies (USA, Canada, Australia, Brazil, EU member states, etc.). Typically have well-documented fire perimeters, cause, size, and suppression data.

### 6. Burned Area & Emissions Databases
Post-fire assessment products measuring the extent of burned land and associated greenhouse gas and aerosol emissions. Useful for impact modelling and trend analysis.
