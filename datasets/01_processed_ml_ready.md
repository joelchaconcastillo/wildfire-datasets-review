# Category 1 — Processed / ML-Ready Datasets

These datasets have been pre-cleaned, structured, and in most cases feature-engineered specifically for machine learning or statistical modelling. They are the fastest path to a working prototype but may lag behind real-time and may be geographically limited.

---

## 1.1 FPA FOD — Fire Program Analysis Fire-Occurrence Database (USA)

**URL:** https://www.fs.usda.gov/rds/archive/Catalog/RDS-2013-0009  
**Maintained by:** USDA Forest Service  
**License:** Public Domain (US Government work)

### Description
The most comprehensive official database of US wildfire occurrences, compiled from reporting systems of federal, state, and local fire organisations. Version 6 (2022 release) covers **1.88 million fire records** from **1992 to 2020**. Each record includes discovery date, containment date, fire size (acres), cause, geographic coordinates (lat/lon), state, county, FIPS code, and the source agency.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Records | ~1.88 million fires |
| Period | 1992–2020 |
| Spatial coverage | Continental USA + Alaska + Hawaii |
| Format | SQLite (.sqlite), CSV |
| Spatial resolution | Point (fire origin lat/lon) |
| Variables | Discovery date, size, cause, location, agency, NWCG fire class (A–G) |

### Strengths
- Longest continuous US fire record with standardised fields
- Includes fire cause (lightning vs. human-ignited) — critical for prediction
- Contains fire size class which enables classification models

### Limitations
- USA only
- No weather or vegetation covariates (must be joined from external sources)
- Point-of-origin only; no fire perimeters

### Relevance for Daily Prediction
Medium. Good for training a US-specific ignition probability model. Requires enrichment with meteorological data (e.g., ERA5 or GRIDMET) for a full prediction pipeline.

---

## 1.2 Kaggle — 1.88 Million US Wildfires

**URL:** https://www.kaggle.com/datasets/rtatman/188-million-us-wildfires  
**Maintained by:** Kaggle Community (sourced from FPA FOD)  
**License:** CC0 (Public Domain)

### Description
A Kaggle-hosted CSV export of the FPA FOD database (see 1.1 above). Provided as a cleaned SQLite file and is one of the most-downloaded wildfire datasets on Kaggle. Widely used in data science competitions and tutorials. Fundamentally the same data as FPA FOD but with a more accessible download interface.

### Relevance for Daily Prediction
Same as FPA FOD (1.1). Useful as a teaching and prototyping dataset.

---

## 1.3 Wildfire Prediction Dataset (Satellite Image Tiles)

**URL:** https://www.kaggle.com/datasets/abdelghaniaaba/wildfire-prediction-dataset  
**Maintained by:** Kaggle Community (derived from NASA Landsat / FIRMS)  
**License:** CC BY 4.0

### Description
A computer-vision dataset containing **~44,000 satellite image tiles** (350×350 pixels, RGB) labelled as "wildfire" or "no wildfire". Images were sampled from high-fire-risk zones in Algeria and North America (2012–2020). Designed for training deep learning classifiers (CNNs, Vision Transformers) rather than traditional tabular models.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Images | ~44,000 tiles |
| Period | 2012–2020 |
| Spatial coverage | Algeria, North America |
| Format | JPEG / PNG image tiles |
| Spatial resolution | ~30 m (Landsat-derived) |
| Labels | Binary: wildfire / no wildfire |

### Strengths
- Plug-and-play for deep learning experiments
- Pre-split into train/validation/test sets

### Limitations
- Geographic scope is limited (not truly global)
- Binary label only; no fire intensity or size information

---

## 1.4 UCI Forest Fires Dataset

**URL:** https://archive.ics.uci.edu/dataset/162/forest+fires  
**Maintained by:** UCI Machine Learning Repository  
**License:** CC BY 4.0

### Description
A small but classic regression dataset from the Montesinho Natural Park in northeast **Portugal**, spanning January 2000 – December 2003. Contains 517 records with spatial coordinates (X, Y on a 9×9 park grid), month, day, FWI system components (FFMC, DMC, DC, ISI), temperature, relative humidity, wind speed, rainfall, and burned area (ha).

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Records | 517 |
| Period | 2000–2003 |
| Spatial coverage | Montesinho Park, Portugal |
| Format | CSV |
| Target variable | Burned area (ha) |

### Strengths
- Benchmark dataset; many published baseline models to compare against
- Includes full FWI system components

### Limitations
- Very small; inadequate for training production models
- Single location in Portugal

---

## 1.5 Global Fire Atlas

**URL:** https://www.globalfireatlas.com/ | Data: https://daac.ornl.gov/cgi-bin/dsviewer.pl?ds_id=1642  
**Maintained by:** NASA ORNL DAAC (Andela et al.)  
**License:** Public Domain

### Description
A global, daily, 500-m resolution fire tracking product derived from MODIS MCD64A1 burned area data. Covers **2003–2016** and provides individual fire event characteristics including ignition date, duration, fire spread rate, maximum fire size, and final perimeter. Each fire is tracked as a dynamic entity across multiple days.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 2003–2016 |
| Spatial coverage | Global |
| Format | NetCDF, GeoTIFF |
| Spatial resolution | 500 m |
| Variables | Ignition date, duration, size, spread rate, final perimeter |

### Strengths
- Only publicly available global fire-tracking product (individual fire events, not just detections)
- Useful for studying fire behavior and spread dynamics
- Peer-reviewed (Andela et al., 2019, *Nature Geoscience*)

### Limitations
- Ends at 2016; not updated
- MODIS-derived → 500 m minimum fire size detection

---

## 1.6 Canadian Wildland Fire Information System (CWFIS) Historical Dataset

**URL:** https://cwfis.cfs.nrcan.gc.ca/datamart  
**Maintained by:** Natural Resources Canada (NRCan)  
**License:** Open Government Licence – Canada

### Description
The Canadian Forest Service provides historical fire weather observations, fire weather indices (FWI system), and fire occurrence data across Canada. The datamart offers downloadable CSV and shapefiles of fire weather station data, fire occurrence polygons, and the M3 Drought Code maps.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 1959–present |
| Spatial coverage | Canada |
| Format | CSV, Shapefile, GeoJSON |
| Variables | FWI system components, fire occurrence polygons |

---

## 1.7 CALFIRE Incident Archive

**URL:** https://www.fire.ca.gov/incidents/  
**Maintained by:** California Department of Forestry and Fire Protection (CAL FIRE)  
**License:** Public Domain (California Government data)

### Description
Official incident archive for California wildfires, updated in near-real-time. Includes fire name, start/end dates, acres burned, containment percentage, county, latitude/longitude of origin. Historical records go back decades and are accessible via downloadable data and an open API.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | Multi-decade (varies) |
| Spatial coverage | California, USA |
| Format | CSV, JSON via API |
| Variables | Name, start/end date, acres, containment %, location |

---

## 1.8 FIRMS — Historical Fire Archive (CSV Downloads)

**URL:** https://firms.modaps.eosdis.nasa.gov/download/  
**Maintained by:** NASA FIRMS  
**License:** Public Domain

### Description
NASA FIRMS provides downloadable archives of **MODIS Collection 6.1** and **VIIRS SNPP/NOAA-20** active fire detections as CSV files, covering the full mission lifetimes (MODIS: 2000–present; VIIRS: 2012–present). Each row represents one active fire pixel with brightness temperature, Fire Radiative Power (FRP), confidence, and geographic coordinates.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | MODIS: 2000–present; VIIRS: 2012–present |
| Spatial coverage | Global |
| Format | CSV, Shapefile |
| Temporal resolution | Daily composites |
| Variables | Lat, lon, brightness, FRP, confidence, satellite, acquisition datetime |

### Strengths
- Longest global active fire record
- Global coverage, daily composites available by country or bounding box
- Free, no registration required for bulk download

---

## 1.9 Monitoring Trends in Burn Severity (MTBS) — Summarised Data

**URL:** https://www.mtbs.gov/direct-download  
**Maintained by:** USGS / USFS  
**License:** Public Domain

### Description
MTBS produces Landsat-derived burn severity maps for all fires ≥ 1,000 acres (western US) and ≥ 500 acres (eastern US) since 1984. The project website provides both raster burn severity mosaics and a tabular fire occurrence dataset with attributes: fire name, year, start date, biome, area, satellite used, and dNBR statistics.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 1984–present (annual updates) |
| Spatial coverage | USA |
| Format | GeoTIFF (rasters), CSV (tabular) |
| Spatial resolution | 30 m |

*See deep-dive analysis in [`../top_sources/mtbs/README.md`](../top_sources/mtbs/README.md)*

---

## 1.10 Next Generation Wildfire Risk Dataset (NDWS)

**URL:** https://huggingface.co/datasets/google/next_day_wildfire_spread  
**Maintained by:** Google Research (Hugging Face Hub)  
**License:** CC BY 4.0 (original paper: Huot et al., 2021)

### Description
A curated multi-variate dataset designed specifically for **next-day wildfire spread prediction**. Assembled from 11 years (2012–2018) of US fire data, it includes satellite (MODIS) fire masks, vegetation (NDVI), topography (elevation, slope, aspect), weather (temperature, humidity, wind, precipitation), and drought index (PDSI) features at 1 km resolution for 64×64 km tiles around active fire events.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 2012–2018 |
| Spatial coverage | Contiguous USA |
| Format | TFRecord (TensorFlow) / HuggingFace datasets |
| Spatial resolution | 1 km tiles |
| Variables | Fire mask, NDVI, EVI, elevation, slope, aspect, PDSI, temperature, humidity, wind, precipitation |

### Strengths
- Purpose-built for ML prediction of fire spread
- Multi-modal (fire + weather + vegetation + topography)
- Peer-reviewed (Huot et al., 2021, *Frontiers in Forests and Global Change*)
- Available directly via HuggingFace `datasets` library

### Limitations
- USA only
- Ends at 2018
- TFRecord format requires TensorFlow/HuggingFace to load conveniently
