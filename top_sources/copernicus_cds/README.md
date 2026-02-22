# Deep-Dive: Copernicus Climate Data Store (CDS) — ERA5 & Fire Danger Indices

**Official Site:** https://cds.climate.copernicus.eu/  
**Maintained by:** ECMWF / Copernicus Climate Change Service (C3S) & Copernicus Emergency Management Service (CEMS)  
**License:** Copernicus License v1.2 (free for any use, including commercial)  
**Overall Rank:** 🥈 #2 for global daily wildfire prediction

---

## Overview

The Copernicus Climate Data Store is the central repository of the European Union's Earth observation programme. For fire research, it provides two critical and complementary products:

1. **ERA5 Reanalysis** — the world's most complete and widely-used global climate reanalysis, providing all meteorological drivers of fire.
2. **Fire Danger Indices (CEMS-Fire-Historical)** — pre-computed Canadian Forest Fire Weather Index (FWI) system components derived from ERA5, eliminating the need to compute fire weather indices from scratch.

Both products are freely available via the `cdsapi` Python package and cover the entire globe at high spatial resolution with an 80-year historical record (ERA5) and 45-year fire weather record (FWI from 1979).

---

## Product 1: ERA5 Reanalysis

### Official Dataset Page
https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels

### Technical Specifications

| Attribute | ERA5 Single Levels | ERA5-Land |
|-----------|-------------------|-----------|
| CDS Dataset ID | `reanalysis-era5-single-levels` | `reanalysis-era5-land` |
| Spatial resolution | 0.25° × 0.25° (~31 km) | 0.1° × 0.1° (~9 km) |
| Temporal resolution | Hourly | Hourly |
| Period | January 1940–present | January 1950–present |
| Latency | 5 days behind real-time | 5 days behind real-time |
| Format | NetCDF, GRIB2 | NetCDF, GRIB2 |
| Vertical levels | 37 pressure levels | Surface only |

### Key Variables for Wildfire Prediction

| Variable | ERA5 Name | Short Name | Unit |
|----------|-----------|------------|------|
| 2m air temperature | `2m_temperature` | t2m | K |
| 2m dew point temperature | `2m_dewpoint_temperature` | d2m | K |
| Total precipitation | `total_precipitation` | tp | m/hour |
| 10m U-wind | `10m_u_component_of_wind` | u10 | m/s |
| 10m V-wind | `10m_v_component_of_wind` | v10 | m/s |
| Volumetric soil water layer 1 (0–7 cm) | `volumetric_soil_water_layer_1` | swvl1 | m³/m³ |
| Volumetric soil water layer 2 (7–28 cm) | `volumetric_soil_water_layer_2` | swvl2 | m³/m³ |
| Leaf area index (high veg) | `leaf_area_index_high_vegetation` | lai_hv | m²/m² |
| Leaf area index (low veg) | `leaf_area_index_low_vegetation` | lai_lv | m²/m² |
| Surface net solar radiation | `surface_net_solar_radiation` | ssr | J/m² |
| Evaporation | `evaporation` | e | m of water eq. |

### Derived Variables for Fire
From ERA5 variables, the following can be computed:
- **Relative Humidity** = f(t2m, d2m) using Magnus formula
- **Wind speed** = sqrt(u10² + v10²)
- **Vapor Pressure Deficit (VPD)** = saturation_vp(t2m) − actual_vp(d2m)
- **Precipitation deficit / anomaly** (standardised = SPEI-like)

---

## Product 2: Fire Danger Indices (CEMS-Fire-Historical)

### Official Dataset Page
https://cds.climate.copernicus.eu/datasets/cems-fire-historical

### Technical Specifications
| Attribute | Detail |
|-----------|--------|
| CDS Dataset ID | `cems-fire-historical` |
| Period | 1979–2022 (historical); extended via CEMS-Fire-Reanalysis |
| Spatial resolution | 0.1° × 0.1° |
| Temporal resolution | Daily |
| Format | NetCDF |
| Global | Yes |

### FWI System Variables Available

| Code | Full Name | Description |
|------|-----------|-------------|
| `ffmc` | Fine Fuel Moisture Code | Moisture content of fine surface fuels; drives ignition probability |
| `dmc` | Duff Moisture Code | Moisture in loosely compacted organic layers (litter, duff) |
| `dc` | Drought Code | Long-term drought moisture deficit in deep compacted organic matter |
| `isi` | Initial Spread Index | Rate of fire spread; function of wind speed and FFMC |
| `bui` | Buildup Index | Total fuel available for combustion; combines DMC and DC |
| `fwi` | Fire Weather Index | Overall fire intensity index; combines ISI and BUI |
| `dsr` | Daily Severity Rating | Non-linear transformation of FWI → suppression difficulty |

### Additional Danger Indices
| Code | Name | Description |
|------|------|-------------|
| `nesterov` | Nesterov Index | Alternative fire danger index (used in Russia/Eastern Europe) |
| `mark5` | Mark 5 Index | Australian McArthur Forest Fire Danger Index (FFDI) |
| `angstrom` | Ångström Index | Swedish fire risk index |

---

## Python API Usage

### Setup
```bash
pip install cdsapi
# Configure credentials in ~/.cdsapirc:
# url: https://cds.climate.copernicus.eu/api/v2
# key: UID:API_KEY  (from https://cds.climate.copernicus.eu/user)
```

### Download Daily FWI (global, full year)
```python
import cdsapi
import xarray as xr

c = cdsapi.Client()

# Download all FWI system components for 2022 (takes ~5-10 min)
c.retrieve(
    'cems-fire-historical',
    {
        'product_type': 'reanalysis',  # or 'intermediate'
        'variable': ['fire_weather_index', 'initial_spread_index',
                     'fine_fuel_moisture_code', 'duff_moisture_code',
                     'drought_code', 'buildup_index'],
        'version': '4.0',
        'dataset': 'Reanalysis',
        'year': '2022',
        'month': [f'{m:02d}' for m in range(1, 13)],
        'day': [f'{d:02d}' for d in range(1, 32)],
        'format': 'zip'
    },
    'fwi_global_2022.zip'
)

# Load and inspect
ds = xr.open_dataset('fwi_global_2022.nc')
print(ds)
```

### Download ERA5 Fire Weather Variables (daily aggregates)
```python
import cdsapi

c = cdsapi.Client()

c.retrieve(
    'reanalysis-era5-land',
    {
        'product_type': 'reanalysis',
        'variable': [
            '2m_temperature', '2m_dewpoint_temperature',
            'total_precipitation',
            '10m_u_component_of_wind', '10m_v_component_of_wind',
            'volumetric_soil_water_layer_1', 'volumetric_soil_water_layer_2',
        ],
        'year': '2023',
        'month': ['06', '07', '08'],  # Summer months
        'day': [f'{d:02d}' for d in range(1, 32)],
        'time': ['12:00'],  # Noon UTC → midday fire weather
        'format': 'netcdf'
    },
    'era5_land_summer_2023.nc'
)
```

---

## ERA5 Climate Trend Analysis

ERA5 provides a uniquely long global record for fire-climate trend analysis:

### Key Trends Documented by Copernicus Reports
- Global mean temperatures 2015–2023 are the 9 warmest years on record
- Vapour Pressure Deficit (VPD) has increased significantly in fire-prone regions (western USA, Mediterranean, Australia)
- Fire seasons are lengthening by ~18–24 days per decade in many regions (Jones et al., 2022)

### Fire Season Metrics Computable from ERA5
1. **Annual sum of FWI > 50 (extreme fire weather days)**
2. **Length of fire season** (first day to last day with FWI > threshold)
3. **Frequency of FWI > 85th percentile events** (trend analysis)
4. **Drought index anomalies** (PDSI, SPEI computed from ERA5)

---

## Copernicus Climate Bulletins

Copernicus issues monthly climate bulletins that include fire weather summaries:
- **C3S Monthly Climate Bulletin:** https://climate.copernicus.eu/climate-bulletins
- **CEMS European Forest Fire Bulletin:** https://effis.jrc.ec.europa.eu/reports-and-publications/fire-news

---

## Forecast Products (for operational prediction)

Beyond reanalysis, CDS also provides forward-looking fire weather:

| Product | Dataset ID | Description |
|---------|------------|-------------|
| Medium-range FWI (10 days) | `cems-fire-forecast` | ECMWF HRES-driven FWI, 10-day, 0.1° |
| Seasonal forecast | `seasonal-original-single-levels` | SEAS5 seasonal ensemble, ~3 months |
| Ensemble fire danger | `cems-fire-forecast` | 51-member ENS FWI probability distribution |

These enable **probabilistic seasonal fire outlooks** — a key product for the potential paper.

---

## Known Limitations

1. **5-day reanalysis delay** — ERA5 is not truly real-time; the latest 5 days are provisional. For real-time applications, use ECMWF HRES (requires licence or TIGGE access).
2. **Spatial resolution** — 0.1° (~9 km for ERA5-Land) is insufficient for local-scale fire behaviour modelling; needs downscaling for high-resolution applications.
3. **FWI startup conditions** — The fire danger indices require spin-up time (DMC and DC depend on previous days); January 1 initial conditions affect winter/spring values.
4. **Tropical fire regimes** — The Canadian FWI system was developed for temperate/boreal forests; it underperforms in tropical savanna and peatland fire environments.

---

## Key Publications

1. Hersbach, H., et al. (2020). The ERA5 global reanalysis. *Quarterly Journal of the Royal Meteorological Society*, 146(730), 1999–2049. https://doi.org/10.1002/qj.3803
2. Vitolo, C., et al. (2019). ERA5-based global meteorological wildfire danger maps. *Scientific Data*, 6, 216. https://doi.org/10.1038/s41597-019-0312-2
3. Jones, M.W., et al. (2022). Global and regional trends and drivers of fire under climate change. *Reviews of Geophysics*, 60(3). https://doi.org/10.1029/2022RG000752

---

## Data Citation
```
Copernicus Climate Change Service (C3S) (2023): ERA5 hourly data on single levels
from 1940 to present. Copernicus Climate Change Service (C3S) Climate Data Store (CDS).
https://doi.org/10.24381/cds.adbb2d47

Vitolo, C., Di Giuseppe, F., Barnard, C., Coughlan, R., San-Miguel-Ayanz, J., Libertà, G.,
& Krzeminska, D. (2019). ERA5-based global meteorological wildfire danger maps [Data set].
Scientific Data. https://doi.org/10.1038/s41597-019-0312-2
```
