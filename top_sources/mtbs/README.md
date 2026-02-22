# Deep-Dive: MTBS — Monitoring Trends in Burn Severity

**Official Site:** https://www.mtbs.gov/  
**Data Download:** https://www.mtbs.gov/direct-download  
**Maintained by:** USGS / USDA Forest Service  
**License:** Public Domain (US Government)  
**Overall Rank:** #5 for global daily wildfire prediction (best US historical severity dataset; critical North American benchmark)

---

## Overview

Monitoring Trends in Burn Severity (MTBS) is a joint USGS/USFS program that systematically maps fire extent and burn severity for all **significant US fires** (≥ 1,000 acres in the Western US; ≥ 500 acres in the Eastern US) using **Landsat imagery** from 1984 to the present. MTBS is the most detailed, longest-running, highest-resolution fire record available for the United States.

MTBS provides:
- **Burn severity raster maps** (GeoTIFF, 30 m resolution) for each fire
- **Fire occurrence tabular data** (fire name, year, size, biome, coordinates)
- **Annual and multi-year burn severity mosaics** for CONUS, Alaska, Hawaii, and Puerto Rico

---

## Why MTBS is Unique

Unlike active fire detection products (MODIS, VIIRS), MTBS maps **post-fire burn severity** — the degree to which vegetation was altered by fire. Burn severity is a key ecological and management indicator:

- **Unburned** → Fire did not affect vegetation
- **Low severity** → Minimal change to overstory; litter and shrubs affected
- **Moderate severity** → Mixed response; patchy overstory mortality
- **High severity** → Complete overstory mortality; maximum vegetation change
- **Increased greenness** → Post-fire productivity pulse (happens in some ecosystems)

This information enables:
1. Estimating carbon emissions more precisely than active fire products
2. Understanding vegetation recovery trajectories
3. Mapping wildland-urban interface fire risk
4. Training models to predict not just fire occurrence but fire impact

---

## Burn Severity Methodology

### Differenced Normalized Burn Ratio (dNBR)
The core MTBS metric is the **dNBR** computed from pre- and post-fire Landsat imagery:

```
NBR = (NIR − SWIR2) / (NIR + SWIR2)
    = (Band 5 − Band 7) / (Band 5 + Band 7)     [Landsat 5/7]
    = (Band 5 − Band 7) / (Band 5 + Band 7)     [Landsat 8/9]

dNBR = NBR_prefire − NBR_postfire
```

| dNBR Value | Burn Severity Class |
|-----------|---------------------|
| < −100 | Enhanced Regrowth (high) |
| −100 to 99 | Enhanced Regrowth (low) / Unburned |
| 100 to 269 | Low severity |
| 270 to 439 | Moderate-low severity |
| 440 to 659 | Moderate-high severity |
| ≥ 660 | High severity |

### Relative dNBR (RdNBR)
MTBS also provides Relative dNBR (RdNBR = dNBR / √|NBR_prefire|) which normalises for pre-fire vegetation condition — more ecologically meaningful in heterogeneous landscapes.

---

## Data Products

### 1. Individual Fire Perimeter + Severity Raster
For every mapped fire, MTBS provides:
- **Burned Area Reflectance Classification (BARC)** raster — dNBR thresholded to 4-6 severity classes
- **dNBR raster** — continuous index (−500 to +1300)
- **RdNBR raster** — relative burn severity
- **Fire perimeter shapefile** — the final mapped boundary
- **Landsat image pair** (pre-fire and post-fire composites)

### 2. Fire Occurrence Table (All Fires)
A CSV/geodatabase table of all mapped fires:

| Field | Type | Description |
|-------|------|-------------|
| `Event_ID` | string | Unique fire identifier (e.g., `CA3760412103920180722`) |
| `Incid_Name` | string | Fire name (e.g., "CARR FIRE") |
| `Incid_Type` | string | "Wildfire" / "Prescribed Fire" |
| `Map_ID` | int | Internal MTBS map identifier |
| `Map_Prog` | string | "CONUS" / "Alaska" / etc. |
| `Asmnt_Type` | string | Assessment type |
| `Pre_ID` | string | Landsat pre-fire scene ID |
| `Post_ID` | string | Landsat post-fire scene ID |
| `Ig_Date` | date | Ignition date |
| `Ig_Year` | int | Ignition year |
| `Low_T` | float | Area (ha) — low severity |
| `Mod_T` | float | Area (ha) — moderate severity |
| `High_T` | float | Area (ha) — high severity |
| `Unb_T` | float | Area (ha) — unburned within perimeter |
| `Total_T` | float | Total mapped area (ha) |
| `R_ENTRMNT` | string | Biome (Temperate/Boreal) |
| `BndryType` | string | Perimeter source type |
| `dNBR_stddev` | float | Spatial variability of burn severity |
| `IncGreen_T` | float | Area (ha) — increased greenness |

### 3. Annual and Multi-Year Burn Severity Mosaics
Annual mosaics of all fires in CONUS, Alaska, Hawaii, Puerto Rico at 30 m resolution. Available from 1984–present.

```
# Direct download links
https://edcintl.cr.usgs.gov/downloads/sciweb1/shared/MTBS_Fire/data/composite_data/burned_area_extent_shapefile/mtbs_perimeter_data.zip
https://edcintl.cr.usgs.gov/downloads/sciweb1/shared/MTBS_Fire/data/composite_data/event_level/annual_burn_severity_mosaics/
```

---

## Statistical Overview

### US Fire History from MTBS (1984–2022)

| Metric | Value |
|--------|-------|
| Total fires mapped | ~35,000 fires |
| Total burned area | ~100 million ha (1984–2022) |
| Annual average burned area | ~2.5–3 million ha |
| Largest fire on record | Bootleg Fire, OR (2021) — 163,000 ha; Dixie Fire, CA (2021) — 387,000 ha |
| Longest CONUS fire season | 2020 (California alone burned >1.7 million ha) |

### Trend Analysis (MTBS 1984–2022)
- **High-severity area has approximately doubled** since 1985 in western US forests
- **Median fire size has increased by ~2x** in the last 30 years
- **Fire season length** (first ignition to last containment) has expanded by ~40% since 1970
- **Western US fire activity 5× higher in 2010s vs 1970s** (Westerling, 2016)

---

## Python Access

### Download and Parse Fire Occurrence Table
```python
import pandas as pd
import geopandas as gpd
import requests
from zipfile import ZipFile
import io

# Download the MTBS fire occurrence points shapefile
url = "https://edcintl.cr.usgs.gov/downloads/sciweb1/shared/MTBS_Fire/data/composite_data/burned_area_extent_shapefile/mtbs_perimeter_data.zip"

response = requests.get(url, stream=True)
with ZipFile(io.BytesIO(response.content)) as z:
    z.extractall("mtbs_perimeters/")

# Load as GeoDataFrame
gdf = gpd.read_file("mtbs_perimeters/mtbs_perims_DD.shp")
print(gdf.columns.tolist())
print(f"Total fires: {len(gdf)}")
print(gdf.groupby('Year')['BurnBndAc'].sum().sort_index().tail(10))
```

### Compute High-Severity Trend
```python
import pandas as pd

# Load tabular data (CSV version)
df = pd.read_csv("mtbs_fire_occurrence.csv")

# Filter wildfires only (exclude prescribed)
wildfires = df[df['Incid_Type'] == 'Wildfire'].copy()

# Annual high-severity area
annual = wildfires.groupby('Ig_Year')['High_T'].sum().reset_index()
annual.columns = ['year', 'high_severity_ha']

# Trend
from scipy import stats
slope, intercept, r, p, se = stats.linregress(annual.year, annual.high_severity_ha)
print(f"High-severity trend: +{slope:,.0f} ha/year (p={p:.4f}, R²={r**2:.3f})")
```

---

## Integration with Other Sources

### MTBS + ERA5 for Fire Severity Prediction
Combining MTBS burn severity maps with ERA5 fire weather at time of fire:
1. Extract ignition date from MTBS fire occurrence table
2. Query ERA5 for FWI/weather on that date at fire location
3. Analyse relationship between fire weather severity and burn severity class

### MTBS + VIIRS for Modern Validation
MTBS provides ground-truth fire perimeters that can validate VIIRS active fire detection:
- For each MTBS fire 2012–present, count VIIRS detections within the perimeter
- Compute detection probability as a function of fire size and severity

### Cross-Reference with FPA FOD
MTBS fires (≥ 500–1,000 acres) can be linked to FPA FOD records (all fires) using fire name, date, and location to create a combined dataset with both precise ignition information (FPA FOD) and burn severity (MTBS).

---

## MTBS for the Potential Paper

For "Wildfire on Earth: Trends, Patterns, and Relevant Factors," MTBS provides:

1. **Strongest US evidence** for increasing fire severity trend (40-year Landsat record)
2. **Biome-level severity analysis** (e.g., high-severity fires increasing faster in ponderosa pine than lodgepole pine)
3. **Historical baseline** for comparing pre- and post-fire management era conditions
4. **WUI (Wildland-Urban Interface) fire impact** quantification

---

## Key Publications

1. **Eidenshink, J., et al. (2007)** — MTBS foundational paper:
   Eidenshink, J., Schwind, B., Brewer, K., Zhu, Z.L., Quayle, B., & Howard, S. (2007). A project for monitoring trends in burn severity. *Fire Ecology*, 3(1), 3–21. https://doi.org/10.4996/fireecology.0301003

2. **Westerling, A.L. (2016)** — MTBS-based warming amplifies wildfire:
   Westerling, A.L. (2016). Increasing western US forest wildfire activity: sensitivity to changes in the timing of spring. *Philosophical Transactions of the Royal Society B*, 371(1696). https://doi.org/10.1098/rstb.2015.0178

3. **Abatzoglou, J.T., & Williams, A.P. (2016)** — Climate attribution of fire:
   Abatzoglou, J.T., & Williams, A.P. (2016). Impact of anthropogenic climate change on wildfire across western US forests. *PNAS*, 113(42), 11770–11775. https://doi.org/10.1073/pnas.1607171113

---

## Data Citation
```
USGS/USFS MTBS Project (2023). Monitoring Trends in Burn Severity (MTBS) Data Access:
Fire Level Geospatial Data. Available at: https://www.mtbs.gov/direct-download
(accessed DATE). A joint project of USDA Forest Service and USGS Earth Resources
Observation and Science (EROS) Center.
```
