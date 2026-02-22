# Deep-Dive: NASA FIRMS — Fire Information for Resource Management System

**Official Site:** https://firms.modaps.eosdis.nasa.gov/  
**Maintained by:** NASA Earth Science Data and Information System (ESDIS)  
**License:** Public Domain (US Government)  
**Overall Rank:** 🥇 #1 for global daily wildfire prediction

---

## Overview

NASA FIRMS is the world's premier operational global fire monitoring system. It ingests, processes, and distributes fire detection data from multiple satellites in near-real-time, providing the critical fire labels for any global fire prediction model. FIRMS is the production system operated 24/7 by NASA and serves thousands of users daily including fire management agencies, governments, and researchers worldwide.

FIRMS integrates three fire detection instruments:
- **MODIS** on Terra and Aqua (2000–present, 1 km resolution)
- **VIIRS** on Suomi-NPP (2012–present, 375 m resolution)
- **VIIRS** on NOAA-20 (2018–present, 375 m resolution)

---

## Data Products

### VIIRS 375m Near-Real-Time (VIIRS_SNPP_NRT, VIIRS_NOAA20_NRT)
The primary recommended product for modern applications.

| Attribute | Detail |
|-----------|--------|
| Spatial resolution | 375 m (I-band thermal) |
| Temporal resolution | Daily; near-real-time within ~3 hours of acquisition |
| Global revisit time | ~2 passes per day (SNPP); ~2 passes per day (NOAA-20) |
| Period | SNPP: January 2012–present; NOAA-20: January 2018–present |
| Data latency | ~3 hours (NRT); 2–7 days (Standard) |

### VIIRS 375m Standard Product (VIIRS_SNPP_SP, VIIRS_NOAA20_SP)
Reprocessed, quality-controlled version with improved geolocation and calibration. Preferred for research and historical analysis.

### MODIS 1km Near-Real-Time (MODIS_NRT, MODIS_SP)
| Attribute | Detail |
|-----------|--------|
| Spatial resolution | 1 km |
| Period | March 2000–present (Terra); July 2002–present (Aqua) |
| Data latency | ~3 hours (NRT) |

---

## Data Schema

Every row in the VIIRS CSV output represents one active fire detection pixel:

| Field | Type | Description |
|-------|------|-------------|
| `latitude` | float | Center latitude of 375 m pixel |
| `longitude` | float | Center longitude of 375 m pixel |
| `bright_ti4` | float | Brightness temperature at ~4 µm (K) — primary fire signal |
| `scan` | float | Along-scan pixel size (km) |
| `track` | float | Along-track pixel size (km) |
| `acq_date` | date | Acquisition date (YYYY-MM-DD) |
| `acq_time` | int | Acquisition time (HHMM UTC) |
| `satellite` | string | "N" (NOAA-20) or "S" (Suomi-NPP) |
| `instrument` | string | "VIIRS" |
| `confidence` | string | "low", "nominal", "high" |
| `version` | string | Algorithm version (e.g., "2.0NRT") |
| `bright_ti5` | float | Brightness temperature at ~11 µm (K) — background |
| `frp` | float | Fire Radiative Power (MW) — proxy for fire intensity |
| `daynight` | char | "D" (daytime) or "N" (nighttime) |

### MODIS additional/different fields:
- `brightness` → 21 µm brightness temperature
- `bright_t31` → 31 µm brightness temperature (replaces `bright_ti5`)
- `confidence` → 0–100 integer (not string categories)

---

## API Access

### Authentication
Register for a free MAP_KEY:  
https://firms.modaps.eosdis.nasa.gov/api/map_key/

### REST Endpoints

```bash
# VIIRS SNPP NRT — last 24 hours, world bounding box
curl "https://firms.modaps.eosdis.nasa.gov/api/area/csv/{MAP_KEY}/VIIRS_SNPP_NRT/world/1"

# VIIRS — country (ISO 3166-1 alpha-3), 7 days
curl "https://firms.modaps.eosdis.nasa.gov/api/country/csv/{MAP_KEY}/VIIRS_SNPP_NRT/BRA/7"

# MODIS — bounding box, 48 hours
# Format: west,south,east,north
curl "https://firms.modaps.eosdis.nasa.gov/api/area/csv/{MAP_KEY}/MODIS_NRT/-130,25,-60,55/2"

# KML (for mapping tools)
curl "https://firms.modaps.eosdis.nasa.gov/api/area/kml/{MAP_KEY}/VIIRS_SNPP_NRT/world/1"
```

### Python Integration
```python
import requests
import pandas as pd
import io

MAP_KEY = "YOUR_KEY_HERE"
product = "VIIRS_SNPP_NRT"
bbox = "-130,25,-60,55"  # Continental USA
days = 7

url = f"https://firms.modaps.eosdis.nasa.gov/api/area/csv/{MAP_KEY}/{product}/{bbox}/{days}"
response = requests.get(url)
df = pd.read_csv(io.StringIO(response.text))
print(df.head())
print(f"Total fire detections: {len(df)}")
```

### Bulk Historical Download
```
# Download complete annual archives (no API key needed)
https://firms.modaps.eosdis.nasa.gov/download/

# Direct FTP access
ftp://nrt3.modaps.eosdis.nasa.gov/FIRMS/
```

---

## Coverage & Validation

### Global Annual Fire Detection Statistics (VIIRS SNPP)
| Year | Global Fire Detections (approx.) |
|------|----------------------------------|
| 2012 | ~16 million |
| 2015 | ~20 million (El Niño year) |
| 2019 | ~19 million (Amazon fires, Australia pre-season) |
| 2020 | ~21 million (Australia Black Summer + global) |
| 2022 | ~17 million |

### Detection Probability by Fire Size
The VIIRS 375 m product detects fires as small as ~0.01 ha under clear-sky conditions.

| Fire size | MODIS 1km detection prob. | VIIRS 375m detection prob. |
|-----------|--------------------------|---------------------------|
| < 0.1 ha | ~5% | ~30% |
| 0.1 – 1 ha | ~30% | ~70% |
| 1 – 10 ha | ~70% | ~90% |
| > 10 ha | ~95% | ~98% |

---

## Known Limitations

1. **Cloud cover** — Thick clouds and heavy smoke can block IR detection. Tropical regions during monsoon season have low data availability.
2. **Commission errors** — Industrial thermal anomalies (smelters, gas flares, volcanoes) can trigger false positives. The confidence field (`nominal`/`high`) helps filter.
3. **Omission errors** — Low-intensity prescribed burns under forest canopy may not be detected.
4. **Temporal sampling** — Polar orbit gives ~2 passes per day; large fires growing rapidly between passes can be missed.
5. **VIIRS scan geometry** — Pixel size increases away from nadir (up to ~750 m × 1500 m at swath edges).

### Recommended Filtering for ML Training
```python
# Filter to high/nominal confidence only (reduces commission errors ~60%)
df_filtered = df[df['confidence'].isin(['nominal', 'high'])]

# Remove known gas flares (optional — NOAA gas flare database)
# Remove volcanically active areas (optional)
```

---

## Integration with Other Sources

### Joining with ERA5/FWI (for feature engineering)
```
FIRMS fire pixel → round to nearest 0.1° grid cell → join ERA5-Land/FWI same day
Requires: temporal alignment (UTC acquisition time → local date)
Tool: xarray, pandas merge_asof, or PostGIS spatial join
```

### Cross-validating with MODIS Burned Area (MCD64A1)
FIRMS NRT fires can be retrospectively validated against monthly burned area from MCD64A1 to estimate detection completeness and omission rates.

---

## Key Publications

1. Giglio, L., Schroeder, W., & Justice, C.O. (2016). The collection 6 MODIS active fire detection algorithm and fire products. *Remote Sensing of Environment*, 178, 31–41. https://doi.org/10.1016/j.rse.2016.02.054
2. Schroeder, W., Oliva, P., Giglio, L., & Csiszar, I.A. (2014). The New VIIRS 375 m active fire detection data product: Algorithm description and initial assessment. *Remote Sensing of Environment*, 143, 85–96. https://doi.org/10.1016/j.rse.2013.12.008
3. Giglio, L., Csiszar, I., & Justice, C.O. (2006). Global distribution and seasonality of active fires as observed with the Terra and Aqua MODIS sensors. *Journal of Geophysical Research: Biogeosciences*, 111(G2). https://doi.org/10.1029/2005JG000142

---

## Data Citation
```
NASA FIRMS (2023). Fire Information for Resource Management System (FIRMS) [Data set].
NASA ESDIS. https://doi.org/10.5067/FIRMS/VIIRS/VNP14IMGT_NRT.002
```
