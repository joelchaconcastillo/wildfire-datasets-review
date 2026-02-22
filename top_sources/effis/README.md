# Deep-Dive: EFFIS — European Forest Fire Information System

**Official Site:** https://effis.jrc.ec.europa.eu/  
**GIS Portal:** https://forest.eea.europa.eu/ | https://maps.effis.emergency.copernicus.eu/  
**Maintained by:** European Commission, Joint Research Centre (JRC), Ispra, Italy  
**License:** Open access — Copernicus License v1.2  
**Overall Rank:** #4 for global daily wildfire prediction (highest-quality regional source for Europe + Mediterranean)

---

## Overview

The European Forest Fire Information System (EFFIS) is the most comprehensive operational fire monitoring and forecasting system in Europe and the Mediterranean basin. Established in 2003 as part of the Copernicus Emergency Management Service (CEMS), EFFIS integrates:

1. **Real-time fire detection** from MODIS and VIIRS satellites
2. **Daily fire danger forecasting** using the FWI system driven by ECMWF weather models
3. **Fire perimeter mapping** from satellite imagery and national agency data
4. **Historical fire statistics** for 43 countries (EU + North Africa + Middle East + Western Balkans + Caucasus)

EFFIS is the European counterpart to GWIS (Global Wildfire Information System) and serves as the basis for EU fire risk policy decisions.

---

## Geographic Scope

EFFIS covers **43 countries** in the pan-European and Mediterranean region:

**EU Member States (27):** All EU countries  
**Candidate/Potential Candidate Countries:** Albania, Bosnia and Herzegovina, Georgia, Kosovo, Montenegro, North Macedonia, Serbia, Turkey, Ukraine  
**Mediterranean Non-EU:** Algeria, Egypt, Jordan, Lebanon, Libya, Morocco, Palestine, Syria, Tunisia  

This scope covers one of the most fire-prone regions in the world, including the Mediterranean basin, which has experienced record fire seasons in 2017, 2018, 2021, and 2022.

---

## Data Products

### 1. Active Fire Map (Near-Real-Time)
- **Source:** MODIS Terra/Aqua + VIIRS SNPP/NOAA-20
- **Update frequency:** Twice daily
- **Available since:** 2000
- **Web portal:** https://effis.jrc.ec.europa.eu/apps/effis.viewer/

### 2. Current Situation (Daily Fire Danger)
Daily FWI danger rating map for the EFFIS region:
- **Low / Moderate / High / Very High / Extreme** danger classes
- Derived from ECMWF high-resolution forecast (10-day outlook)
- Displayed as EFFIS-defined danger classes (5 levels)

### 3. Fire History Database (EFFIS Burned Area)
- **Period covered:** 2000–present (systematic); some countries back to 1980s
- **Spatial data type:** Mapped perimeters (polygons) from satellite + national sources
- **Minimum mapping unit:** ~30 ha
- **Attributes:** Country, year, date, area (ha), confidence (satellite / national confirmed)

### 4. Fire Weather Index Forecast (10-day)
- Based on ECMWF HRES 10-day forecast
- Updated daily at 06:00 UTC
- Components: FFMC, DMC, DC, ISI, BUI, FWI, DSR

### 5. Seasonal Outlook
EFFIS issues monthly fire season outlooks for Europe based on seasonal weather forecast ensembles (ECMWF SEAS5). Provides probability of above-normal fire danger by region.

---

## OGC Web Services

EFFIS exposes data through standard OGC Web Map Service (WMS) and Web Feature Service (WFS) interfaces.

### WMS (Map imagery)
```
Base URL: https://maps.effis.emergency.copernicus.eu/geoserver/gwis/wms

# Example: Current fire danger map as PNG image
https://maps.effis.emergency.copernicus.eu/geoserver/gwis/wms?
  service=WMS&version=1.1.1&request=GetMap
  &layers=gwis:fwi_mean_geff&styles=
  &bbox=-30,25,45,75&width=800&height=600&srs=EPSG:4326
  &format=image/png
```

### WFS (Feature data as GeoJSON/GML)
```
Base URL: https://maps.effis.emergency.copernicus.eu/geoserver/gwis/wfs

# Active fires (VIIRS, last 24 hours) as GeoJSON
GET https://maps.effis.emergency.copernicus.eu/geoserver/gwis/wfs?
  service=WFS&version=2.0.0&request=GetFeature
  &typeNames=gwis:viirs_fires_24h
  &outputFormat=application/json

# Burned area perimeters (2022)
GET https://maps.effis.emergency.copernicus.eu/geoserver/gwis/wfs?
  service=WFS&version=2.0.0&request=GetFeature
  &typeNames=gwis:modis_ba_2022
  &outputFormat=application/json
  &CQL_FILTER=area_ha>100
```

### Available WFS Layers (Selection)
| Layer Name | Description |
|-----------|-------------|
| `gwis:viirs_fires_24h` | VIIRS active fires, last 24 hours |
| `gwis:viirs_fires_48h` | VIIRS active fires, last 48 hours |
| `gwis:modis_fires_24h` | MODIS active fires, last 24 hours |
| `gwis:fwi_mean_geff` | Current mean FWI |
| `gwis:effis_ba_YYYY` | Annual burned area perimeters (YYYY = year) |
| `gwis:ba_forecast` | Burned area forecast |

---

## Python Integration

```python
import requests
import geopandas as gpd
from io import BytesIO

# Fetch active fires (last 48h) as GeoJSON
wfs_url = "https://maps.effis.emergency.copernicus.eu/geoserver/gwis/wfs"
params = {
    "service": "WFS",
    "version": "2.0.0",
    "request": "GetFeature",
    "typeNames": "gwis:viirs_fires_48h",
    "outputFormat": "application/json",
    "count": 1000  # max features
}

response = requests.get(wfs_url, params=params)
gdf = gpd.read_file(BytesIO(response.content))
print(f"Active fires in EFFIS region (last 48h): {len(gdf)}")
print(gdf.columns.tolist())
print(gdf.head())
```

---

## EFFIS Historical Statistics

### Annual Burned Area in Europe + Mediterranean (2000–2022)

| Decade | Avg Annual Burned Area |
|--------|------------------------|
| 2000–2009 | ~750,000 ha/year |
| 2010–2019 | ~650,000 ha/year |
| 2020–2022 | ~500,000–900,000 ha/year (high variability) |

### Record Fire Seasons (EFFIS data)
| Year | Notable Events | Burned Area (approx.) |
|------|---------------|----------------------|
| 2007 | Greece catastrophic fires (Peloponnese) | ~420,000 ha in Greece alone |
| 2017 | Portugal (Pedrógão Grande tragedy) | >500,000 ha in Portugal |
| 2021 | Greece (Evia island) + Turkey | >1 million ha |
| 2022 | France, Spain, Portugal summer heat waves | ~800,000 ha |

---

## Country-Level Database Access

EFFIS provides downloadable country-level annual statistics:
```
https://effis.jrc.ec.europa.eu/reports-and-publications/fire-statistics
```

### Download Options
- **Annual summary CSV** per country (fire count, burned area, fire danger statistics)
- **Fire history map** (GIS layers, available on request)
- **Burned area grid** (NetCDF for selected years)

---

## Integration with Global Model

For a **global daily prediction model**, EFFIS adds value as:
1. **Validation benchmark** — compare model predictions against EFFIS official statistics for EU countries
2. **Training data enrichment** — EFFIS fire perimeters provide higher-quality fire labels for Europe than satellite-only products
3. **Feature engineering** — EFFIS daily FWI maps can be used directly as model features for the European region

### Merging EFFIS with FIRMS (for Europe)
```python
import pandas as pd
import geopandas as gpd
from shapely.geometry import Point

# FIRMS data for Europe
firms = pd.read_csv("firms_europe_2023.csv")
firms_gdf = gpd.GeoDataFrame(
    firms,
    geometry=[Point(xy) for xy in zip(firms.longitude, firms.latitude)],
    crs="EPSG:4326"
)

# Load EFFIS fire perimeters (WFS or downloaded shapefile)
effis = gpd.read_file("effis_ba_2023.geojson")

# Spatial join: which FIRMS detections fall within EFFIS perimeters?
joined = gpd.sjoin(firms_gdf, effis, how="left", predicate="within")
confirmed_fires = joined[joined.index_right.notna()]
print(f"FIRMS detections confirmed by EFFIS: {len(confirmed_fires)}/{len(firms_gdf)}")
```

---

## EFFIS Fire News and Reports
- Monthly bulletins during fire season: https://effis.jrc.ec.europa.eu/reports-and-publications/fire-news
- Annual European Forest Fire Report: https://effis.jrc.ec.europa.eu/reports-and-publications/annual-fire-reports
- Ad-hoc country fact sheets (e.g., post-season Portugal, Greece)

---

## Key Publications

1. San-Miguel-Ayanz, J., et al. (2013). Comprehensive monitoring of wildfires in Europe: The European Forest Fire Information System (EFFIS). *Approaches to Managing Disaster*. InTech. https://doi.org/10.5772/51275
2. Durrant, T., et al. (2022). Forest fires in Europe, Middle East and North Africa 2021. EUR 31269 EN, Publications Office of the European Union. https://doi.org/10.2760/039729
3. Barbosa, P., et al. (2009). Assessment of Forest Fire Impacts and Emissions in the European Union Based on the European Forest Fire Information System. In *Wildland Fires and Air Pollution*, Elsevier.

---

## Data Citation
```
San-Miguel-Ayanz, J., Durrant, T., Boca, R., Maianti, P., Liberta, G., Artes Vivancos, T.,
Jacome Felix Oom, D., Branco, A., De Rigo, D., Ferrari, D., Pfeiffer, H., Grecchi, R.,
Onida, M., González Vega, J.A.: Forest Fires in Europe, Middle East and North Africa YEAR.
EUR NNNNN EN, doi:10.2760/XXXXX, Publications Office of the European Union, YEAR.

EFFIS data via: https://effis.jrc.ec.europa.eu
```
