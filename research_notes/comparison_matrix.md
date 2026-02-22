# Comparison Matrix — All Wildfire Data Sources

A side-by-side feature comparison of all catalogued sources to support data source selection for specific use cases.

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Yes / Supported |
| ❌ | No / Not supported |
| ⚠️ | Partial / Limited |
| 🌍 | Global |
| 🌎 | Americas |
| 🌏 | Asia-Pacific |
| 🇪🇺 | Europe / Mediterranean |
| 🇺🇸 | USA only |
| 🇧🇷 | Brazil only |
| 🇨🇦 | Canada only |
| 🇦🇺 | Australia only |

---

## Table 1: Fire Occurrence & Detection Products

| Source | Category | Spatial Res. | Temporal Res. | Coverage | Period | API | Free | Fire Labels | FRP | Perimeters |
|--------|----------|-------------|----------------|----------|--------|-----|------|-------------|-----|-----------|
| [NASA FIRMS VIIRS 375m](../top_sources/nasa_firms/README.md) | Active Fire | 375 m | Daily / NRT | 🌍 | 2012–now | ✅ | ✅ | ✅ | ✅ | ❌ |
| [NASA FIRMS MODIS 1km](../top_sources/nasa_firms/README.md) | Active Fire | 1 km | Daily / NRT | 🌍 | 2000–now | ✅ | ✅ | ✅ | ✅ | ❌ |
| [GOES-16 FDC](../datasets/02_satellite_remote_sensing.md) | Active Fire | ~2 km | 1–15 min | 🌎 | 2017–now | ✅ | ✅ | ✅ | ✅ | ❌ |
| [Himawari-8](../datasets/02_satellite_remote_sensing.md) | Active Fire | 2 km | 10 min | 🌏 | 2015–now | ⚠️ | ✅ | ✅ | ❌ | ❌ |
| [MTBS](../top_sources/mtbs/README.md) | Burn Severity | 30 m | Annual | 🇺🇸 | 1984–now | ❌ | ✅ | ✅ | ❌ | ✅ |
| [WFIGS / NIFC](../datasets/05_regional_national.md) | Perimeters | Mapped | Near-RT | 🇺🇸 | 2019–now | ✅ | ✅ | ✅ | ❌ | ✅ |
| [EFFIS Fire History](../top_sources/effis/README.md) | Perimeters | Mapped | Annual | 🇪🇺 | 1980–now | ✅ | ✅ | ✅ | ❌ | ✅ |
| [FPA FOD](../datasets/01_processed_ml_ready.md) | Occurrence | Point | Daily (hist.) | 🇺🇸 | 1992–2020 | ❌ | ✅ | ✅ | ❌ | ❌ |
| [CNFDB](../datasets/05_regional_national.md) | Occurrence | Point/Polygon | Annual | 🇨🇦 | 1959–now | ❌ | ✅ | ✅ | ❌ | ⚠️ |
| [INPE BDQueimadas](../datasets/05_regional_national.md) | Active Fire | Point | Daily / NRT | 🇧🇷 | 1998–now | ✅ | ✅ | ✅ | ❌ | ❌ |
| [CAL FIRE Incidents](../datasets/01_processed_ml_ready.md) | Occurrence | Point | Near-RT | 🇺🇸 (CA) | Multi-decade | ✅ | ✅ | ✅ | ❌ | ✅ |
| [Global Fire Atlas](../datasets/01_processed_ml_ready.md) | Fire Events | 500 m | Daily | 🌍 | 2003–2016 | ❌ | ✅ | ✅ | ✅ | ✅ |

---

## Table 2: Burned Area & Emissions Products

| Source | Category | Spatial Res. | Temporal Res. | Coverage | Period | API | Free | BA | CO₂ | BC/OC | Biome |
|--------|----------|-------------|----------------|----------|--------|-----|------|-----|-----|-------|-------|
| [GFED 4.1s](../top_sources/global_fire_emissions_database/README.md) | BA + Emissions | 0.25° | Monthly | 🌍 | 1997–2020 | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| [MCD64A1 v061](../datasets/06_burned_area_emissions.md) | Burned Area | 500 m | Monthly | 🌍 | 2000–now | ⚠️ | ✅ | ✅ | ❌ | ❌ | ❌ |
| [ESA Fire_cci v5.1](../datasets/06_burned_area_emissions.md) | Burned Area | 250 m | Monthly | 🌍 | 2001–2020 | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ |
| [GFAS (CAMS)](../datasets/06_burned_area_emissions.md) | Emissions | 0.1° | Daily | 🌍 | 2003–now | ✅ | ✅ | ⚠️ | ✅ | ✅ | ❌ |
| [FINN (NCAR)](../datasets/06_burned_area_emissions.md) | Emissions | 1 km | Daily | 🌍 | 2002–now | ❌ | ✅ | ❌ | ✅ | ✅ | ✅ |
| [SIPONGI (KLHK)](../datasets/06_burned_area_emissions.md) | Active Fire | Point | Daily | 🇮🇩 | 2010–now | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |

---

## Table 3: Climate & Weather Feature Sources

| Source | Type | Spatial Res. | Temporal Res. | Coverage | Period | API | Free | FWI | T, RH, Wind | Precip | Drought | VPD |
|--------|------|-------------|----------------|----------|--------|-----|------|-----|------------|--------|---------|-----|
| [ERA5](../top_sources/copernicus_cds/README.md) | Reanalysis | 0.25° | Hourly | 🌍 | 1940–now | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| [ERA5-Land](../top_sources/copernicus_cds/README.md) | Reanalysis | 0.1° | Hourly | 🌍 | 1950–now | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ |
| [CEMS-Fire FWI](../top_sources/copernicus_cds/README.md) | Fire Weather | 0.1° | Daily | 🌍 | 1979–now | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ |
| [GRIDMET](../datasets/04_climate_weather.md) | Meteorology | ~4 km | Daily | 🇺🇸 | 1979–now | ✅ | ✅ | ⚠️ | ✅ | ✅ | ✅ | ✅ |
| [GFAS FRP](../datasets/06_burned_area_emissions.md) | Fire + Climate | 0.1° | Daily | 🌍 | 2003–now | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| [PDSI (CRU)](../datasets/04_climate_weather.md) | Drought | 0.5° | Monthly | 🌍 | 1901–now | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| [SPEI (CSIC)](../datasets/04_climate_weather.md) | Drought | 0.5° | Monthly | 🌍 | 1901–now | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| [NOAA NCEI CDO](../datasets/04_climate_weather.md) | Observations | Station | Daily | 🌍 | 1763–now | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ |
| [MOD11A1 LST](../datasets/04_climate_weather.md) | Satellite | 1 km | Daily | 🌍 | 2000–now | ⚠️ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |

---

## Table 4: ML-Ready / Pre-processed Datasets

| Source | Type | Size | Format | Coverage | Period | Target Variable | Free |
|--------|------|------|--------|----------|--------|-----------------|------|
| [FPA FOD v6](../datasets/01_processed_ml_ready.md) | Tabular | 1.88M records | SQLite, CSV | 🇺🇸 | 1992–2020 | Occurrence, cause, size | ✅ |
| [Kaggle 1.88M US Wildfires](../datasets/01_processed_ml_ready.md) | Tabular | 1.88M records | SQLite | 🇺🇸 | 1992–2015 | Occurrence | ✅ |
| [NDWS (Google/HuggingFace)](../datasets/01_processed_ml_ready.md) | Raster Tiles | ~18K events | TFRecord | 🇺🇸 | 2012–2018 | Spread (binary) | ✅ |
| [Wildfire Prediction (Kaggle)](../datasets/01_processed_ml_ready.md) | Image Tiles | ~44K images | JPEG/PNG | USA+Algeria | 2012–2020 | Binary class | ✅ |
| [UCI Forest Fires](../datasets/01_processed_ml_ready.md) | Tabular | 517 records | CSV | 🇵🇹 | 2000–2003 | Burned area (ha) | ✅ |
| [Global Fire Atlas](../datasets/01_processed_ml_ready.md) | Raster + Table | Global events | NetCDF | 🌍 | 2003–2016 | Fire behavior | ✅ |
| [CWFIS Historical](../datasets/01_processed_ml_ready.md) | Tabular + Raster | National | CSV, Shapefile | 🇨🇦 | 1959–now | FWI, occurrence | ✅ |

---

## Table 5: Use-Case Suitability Matrix

| Use Case | Best Sources |
|----------|-------------|
| **Global daily fire prediction model (labels)** | NASA FIRMS VIIRS NRT → MODIS for pre-2012 |
| **Fire weather features (global, daily)** | Copernicus CDS ERA5-Land + CEMS-Fire FWI |
| **Historical burned area (training features)** | GFED 4.1s (monthly) → MCD64A1 for 500m |
| **US-specific high-detail modelling** | FPA FOD + MTBS + GRIDMET + WFIGS |
| **European regional model** | EFFIS + ERA5 + CEMS-Fire FWI |
| **Tropical fire (Amazon, SE Asia)** | INPE BDQueimadas + SIPONGI + VIIRS SNPP |
| **Deep learning / image classification** | NDWS (HuggingFace) + Wildfire Prediction (Kaggle) |
| **Long-term trend analysis (paper)** | GFED 4.1s + MCD64A1 + ERA5 + PDSI/SPEI |
| **Fire emissions / carbon accounting** | GFED 4.1s primary; GFAS for daily + recent |
| **Burn severity analysis** | MTBS (USA); Sentinel-2 dNBR (global, 2015+) |
| **Real-time fire perimeter tracking (USA)** | WFIGS / NIFC ArcGIS REST API |
| **Sub-hourly fire monitoring (Americas)** | GOES-16/18 FDC (AWS S3) |
| **Air quality / smoke modelling** | GFAS + FINN + NOAA HRRR-Smoke |

---

## Summary Scores for Global Daily Prediction (1–5 scale)

| Source | Global Coverage | Temporal Res. | Historical Depth | API Access | Variable Richness | **Total (max 25)** |
|--------|----------------|----------------|-----------------|------------|-------------------|--------------------|
| NASA FIRMS VIIRS | 5 | 5 | 4 | 5 | 3 | **22** |
| Copernicus CDS ERA5+FWI | 5 | 5 | 5 | 5 | 5 | **25** |
| GFED 4.1s | 5 | 3 | 5 | 2 | 5 | **20** |
| EFFIS | 3 | 4 | 4 | 4 | 4 | **19** |
| MTBS | 1 | 2 | 5 | 2 | 5 | **15** |
| MODIS MCD64A1 | 5 | 3 | 5 | 3 | 2 | **18** |
| GRIDMET | 1 | 5 | 4 | 4 | 5 | **19** |
| Global Fire Atlas | 5 | 3 | 3 | 1 | 5 | **17** |
| NDWS (Google) | 1 | 3 | 3 | 3 | 5 | **15** |
| GOES-16 FDC | 3 | 5 | 3 | 4 | 3 | **18** |

---

## Recommended Minimum Viable Dataset Stack

For a prototype global daily wildfire prediction model with minimal infrastructure:

### Phase 1: Fast Start (< 1 week setup)
1. **Fire labels:** NASA FIRMS bulk download CSV (VIIRS SNPP, 2012–2023)
2. **Weather features:** Copernicus CDS ERA5-Land download (daily t2m, tp, u10, v10, swvl1)
3. **Fire weather:** CDS CEMS-Fire FWI (daily fwi, ffmc, dmc, dc, isi)
4. **Grid:** 0.1° × 0.1° global grid (~1.3 million land cells)

### Phase 2: Feature Enrichment (1–4 weeks additional)
5. **Vegetation:** MODIS MOD13A2 NDVI (16-day, 1 km → resampled)
6. **Topography:** Copernicus DEM 30m → slope, aspect, elevation at 0.1° mean
7. **Land cover:** ESA WorldCover 2021 → dominant class per 0.1° cell
8. **Historical burn frequency:** GFED 4.1s monthly burned fraction (1997–2020 climatology)

### Phase 3: Regional Enrichment (4–8 weeks additional)
9. **US validation:** MTBS perimeters (2012–2022) + WFIGS current fires
10. **EU validation:** EFFIS burned area perimeters (2012–2022)
11. **Brazil:** INPE BDQueimadas archive
12. **Canada:** CNFDB fire occurrence

Total estimated data volume: ~500 GB compressed (dominated by MODIS + ERA5)
