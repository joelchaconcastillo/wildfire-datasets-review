# Category 4 — Climate & Weather Data

Wildfire occurrence and behavior are driven primarily by weather and climate. This category covers the key meteorological, hydrological, and climate data sources that serve as feature inputs for fire prediction models. All sources here are officially maintained by national or international meteorological agencies.

---

## 4.1 ERA5 — ECMWF Reanalysis v5

**URL:** https://www.ecmwf.int/en/forecasts/datasets/reanalysis-datasets/era5  
**Data Access:** https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels  
**Maintained by:** ECMWF (European Centre for Medium-Range Weather Forecasts)  
**License:** Copernicus License v1.2 (free for any use)

### Description
ERA5 is the fifth generation ECMWF atmospheric reanalysis of the global climate. It provides **hourly estimates** of a large number of atmospheric, land, and oceanic climate variables at **0.25° × 0.25°** (approximately 31 km) global coverage. ERA5 covers the full Earth from the surface to 1 hPa (approximately 80 km altitude) and back to **January 1940** (preliminary) or **January 1979** (full quality).

### Key Variables for Fire Prediction
| Variable | ERA5 Short Name | Relevance |
|----------|----------------|-----------|
| 2m air temperature | t2m | Fuel drying, heat stress |
| 2m dew point temperature | d2m | Relative humidity (derived) |
| Total precipitation | tp | Fuel moisture |
| 10m U-wind component | u10 | Fire spread |
| 10m V-wind component | v10 | Fire spread |
| Soil water layer 1 (0–7 cm) | swvl1 | Vegetation moisture |
| Evapotranspiration | e | Drought proxy |
| Surface solar radiation | ssrd | Fire weather |
| Snow depth | sd | Seasonal risk windows |

### Technical Specifications
| Attribute | Detail |
|-----------|--------|
| Period | 1940–present (5-day delay) |
| Spatial resolution | 0.25° (~31 km) |
| Temporal resolution | Hourly |
| Vertical levels | 37 pressure levels |
| Format | NetCDF, GRIB2 |

### Strengths
- The most widely used reanalysis for climate-driven fire research
- Consistent 80-year record enables robust trend analysis
- Freely accessible via CDS API (Python `cdsapi` package)
- Used by Copernicus FWI computation system

*See also:* [Copernicus CDS deep-dive](../top_sources/copernicus_cds/README.md)

---

## 4.2 ERA5-Land

**URL:** https://cds.climate.copernicus.eu/datasets/reanalysis-era5-land  
**Maintained by:** ECMWF / Copernicus  
**License:** Copernicus License v1.2

### Description
ERA5-Land is a replay of the land component of ERA5 at an enhanced resolution of **0.1° × 0.1°** (~9 km). It focuses on land surface variables (soil temperature, volumetric soil water, snow, runoff, evaporation) and is particularly useful for deriving surface-level fire weather indices and vegetation moisture stress at higher resolution than standard ERA5.

### Key Variables for Fire
- `volumetric_soil_water_layer_1` to `_4` — Multi-depth soil moisture
- `skin_temperature` — Surface temperature
- `leaf_area_index_high/low_vegetation` — LAI proxy for fuel load
- `fraction_of_absorbed_photosynthetically_active_radiation` — FAPAR, vegetation health

---

## 4.3 Copernicus Fire Danger Indices (CEMS-Fire-Historical)

**URL:** https://cds.climate.copernicus.eu/datasets/cems-fire-historical  
**Maintained by:** Copernicus Emergency Management Service (CEMS) / ECMWF  
**License:** Copernicus License v1.2

### Description
Pre-computed daily Fire Weather Index (FWI) system components based on ERA5 meteorological inputs. This is the most direct climate product for fire prediction, providing the complete Canadian Forest Fire Weather Index System at global scale.

### FWI System Components
| Index | Full Name | What it measures |
|-------|-----------|-----------------|
| FFMC | Fine Fuel Moisture Code | Moisture in fine surface fuels (litter, grass) |
| DMC | Duff Moisture Code | Moisture in loosely compacted organic layers |
| DC | Drought Code | Moisture deficit in deep organic layers (seasonal drought) |
| ISI | Initial Spread Index | Expected rate of fire spread (wind + fine fuel moisture) |
| BUI | Buildup Index | Total fuel available for combustion (DMC + DC) |
| FWI | Fire Weather Index | Overall fire intensity rating (ISI + BUI) |
| DSR | Daily Severity Rating | Effort to control a fire (non-linear FWI transform) |

### Technical Specifications
| Attribute | Detail |
|-----------|--------|
| Period | 1979–present |
| Spatial resolution | 0.1° |
| Temporal resolution | Daily |
| Format | NetCDF |
| Global | Yes |

### Relevance for Daily Prediction
**Very High.** The FWI system is the most widely used fire danger rating system globally and is the standard input for fire probability and spread models. Pre-computed FWI from ERA5 saves significant preprocessing effort.

---

## 4.4 GRIDMET — Gridded Surface Meteorological Dataset (USA)

**URL:** https://www.climatologylab.org/gridmet.html  
**Data Access:** https://www.northwestknowledge.net/metdata/data/  
**Maintained by:** University of Idaho Climatology Lab  
**License:** CC0 (Public Domain)

### Description
GRIDMET provides daily high-spatial-resolution (~4 km / 1/24°) surface meteorological data for the **contiguous United States**. It is produced by statistically merging PRISM climate data with NLDAS reanalysis. Particularly valued for fire applications because it includes direct fire weather variables: **Energy Release Component (ERC)** and **Burning Index (BI)** from the National Fire Danger Rating System (NFDRS).

### Key Variables for Fire
| Variable | ID | Description |
|----------|-----|-------------|
| Max temperature | tmmx | °C |
| Min relative humidity | rmin | % |
| Wind speed | vs | m/s |
| Wind direction | th | degrees |
| ERC (fuel model G) | erc | Energy Release Component |
| Burning Index | bi | Burning Index |
| 100-hr dead fuel moisture | fm100 | % |
| 1,000-hr dead fuel moisture | fm1000 | % |
| Vapor pressure deficit | vpd | kPa |

### Access
```python
import xarray as xr
# Daily max temperature 2023
url = "http://thredds.northwestknowledge.net:8080/thredds/dodsC/agg_met_tmmx_1979_CurrentYear_CONUS.nc"
ds = xr.open_dataset(url, engine='netcdf4')
```

---

## 4.5 PDSI — Palmer Drought Severity Index (NOAA/NCEI)

**URL:** https://www.ncei.noaa.gov/access/monitoring/climate-at-a-glance/global/time-series  
**Global PDSI Grid:** https://crudata.uea.ac.uk/cru/data/drought/  
**Maintained by:** NOAA NCEI + Climatic Research Unit (CRU), University of East Anglia  
**License:** Open (research use)

### Description
The Palmer Drought Severity Index (PDSI) is a standardised measure of meteorological drought integrating precipitation and temperature anomalies. It is a critical predictor of large fire years globally. The **scPDSI** (self-calibrated PDSI) from CRU is available as a global gridded monthly product at 0.5° resolution back to 1901.

### Technical Specifications
| Attribute | Detail |
|-----------|--------|
| Period | 1901–present (CRU TS 4.x) |
| Spatial resolution | 0.5° (~55 km) |
| Temporal resolution | Monthly |
| Format | NetCDF |

---

## 4.6 ECMWF Global Fire Assimilation System (GFAS)

**URL:** https://ads.atmosphere.copernicus.eu/datasets/cams-global-fire-emissions-gfas  
**Maintained by:** ECMWF / Copernicus Atmosphere Monitoring Service (CAMS)  
**License:** Copernicus License v1.2

### Description
GFAS assimilates MODIS fire radiative power (FRP) observations to produce **daily global fire emission estimates** including CO, CO₂, particulate matter, aerosols, and trace gases. Unlike GFED (which is monthly), GFAS provides daily emissions at 0.1° resolution, making it valuable for atmospheric chemistry and air quality modelling in fire-prone regions.

### Key Variables
- CO₂, CO, CH₄, PM2.5, NOx emissions (kg m⁻² s⁻¹)
- Dry matter burned (kg m⁻² s⁻¹)
- Fire radiative power (W m⁻²)

---

## 4.7 MODIS Terra/Aqua Land Surface Temperature (MOD11A1 / MYD11A1)

**URL:** https://lpdaac.usgs.gov/products/mod11a1v061/  
**Maintained by:** NASA LP DAAC  
**License:** Public Domain

### Description
Daily 1-km Land Surface Temperature (LST) products from MODIS. Daytime and nighttime LST are critical for estimating surface heating, evapotranspiration, and drought stress. The product provides both day and night temperature plus emissivity for each pixel, along with quality flags.

### Relevance
LST anomalies are useful as pre-fire indicators of drought and heat stress in vegetation. Correlation between elevated daytime LST and subsequent fire occurrence is well-established in the literature.

---

## 4.8 SPEI Global Drought Monitor

**URL:** https://spei.csic.es/database.html  
**Maintained by:** CSIC (Spanish National Research Council)  
**License:** Open for research

### Description
The Standardised Precipitation-Evapotranspiration Index (SPEI) is a multi-scalar drought index that incorporates both precipitation deficit and atmospheric evapotranspiration demand (temperature effect). Available as a global monthly gridded product at 0.5° resolution from 1901–present, at multiple timescales (1, 3, 6, 12, 24, 48 months).

### Relevance for Fire Research
SPEI-3 and SPEI-6 (3-month and 6-month timescales) are strong predictors of fire season severity. Negative SPEI values (drought conditions) correlate with elevated fire risk across all biomes.
