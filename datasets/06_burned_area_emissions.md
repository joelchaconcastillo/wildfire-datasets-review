# Category 6 — Burned Area & Emissions Databases

Post-fire impact databases quantify the spatial extent of burned land and the associated greenhouse gas, aerosol, and trace gas emissions released into the atmosphere. These products are essential for climate modelling, carbon accounting, and trend analysis in the potential paper on "Wildfire on Earth: Trends, Patterns, and Relevant Factors."

---

## 6.1 Global Fire Emissions Database (GFED 4.1s / GFED 5)

**URL:** https://www.globalfiredata.org/  
**Data Download:** https://www.globalfiredata.org/data.html  
**Maintained by:** GFED Science Team (van der Werf et al., VU Amsterdam)  
**License:** Open (research and educational use)

### Description
GFED is the most widely cited global fire emissions database. It combines satellite-derived burned area (primarily MODIS MCD64A1 with small fire augmentation), vegetation models (CASA-GFED), and emission factors to compute monthly fire emissions globally.

**GFED 4.1s** (current standard) covers 1997–2020 and includes "small fires" sampled from MODIS active fire observations. **GFED 5** (beta/new release) updates the burned area algorithm and extends to more recent years.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 1997–2020 (GFED4.1s); extended in GFED5 |
| Spatial resolution | 0.25° (~28 km) |
| Temporal resolution | Monthly |
| Spatial coverage | Global |
| Format | HDF5, NetCDF |

### Variables
| Variable | Description |
|----------|-------------|
| Burned area | Monthly ha burned per grid cell |
| Dry matter burned (DM) | kg m⁻² month⁻¹ |
| CO₂ emissions | g C m⁻² month⁻¹ |
| CO emissions | g CO m⁻² month⁻¹ |
| CH₄, N₂O, NOx, NMHC | Trace gas emissions |
| Black carbon (BC), Organic carbon (OC) | Aerosol emissions |
| Biome-level breakdown | Tropical forests, savannas, extratropical forests, peatlands, croplands, deforestation |

### Strengths
- Gold standard for global fire carbon accounting
- Partitioned by biome and region (enables paper on trends)
- Peer-reviewed (van der Werf et al., 2017, *Earth System Science Data*)
- 23-year globally consistent record

### Limitations
- Monthly temporal resolution; not suitable for real-time applications
- MODIS-based; may undercount small fires in some regions

*See deep-dive in [`../top_sources/global_fire_emissions_database/README.md`](../top_sources/global_fire_emissions_database/README.md)*

---

## 6.2 MODIS Burned Area — MCD64A1 Collection 6.1

**URL:** https://lpdaac.usgs.gov/products/mcd64a1v061/  
**Maintained by:** NASA LP DAAC  
**License:** Public Domain

*(Full description in [02_satellite_remote_sensing.md](02_satellite_remote_sensing.md))*

The primary input to GFED. Monthly 500-m global burned area product. Provides burn date within the month (Julian day), burn date uncertainty, and QA layer. Accessible via NASA AppEEARS for spatial/temporal subsets and via LP DAAC bulk download.

### Access via AppEEARS
```
# Access point (no code, browser-based submission)
https://appeears.earthdatacloud.nasa.gov/

# Bulk download via earthdata-download tool
earthdata download MCD64A1.061 --bbox=-180,-90,180,90 --start=2000-01-01 --end=2023-12-31
```

---

## 6.3 ESA Fire CCI (Climate Change Initiative Burned Area)

**URL:** https://climate.esa.int/en/projects/fire/  
**Data Catalogue:** https://catalogue.ceda.ac.uk/uuid/58f00d8814064b79a0c49662ad3af537  
**Maintained by:** ESA / Copernicus  
**License:** CC BY 4.0

*(Full description in [02_satellite_remote_sensing.md](02_satellite_remote_sensing.md))*

The Fire_cci product at 250 m / monthly provides an independent estimate of global burned area (2001–2020) with uncertainty bounds. Includes land cover classification of burned pixels — critical for biome-level analysis.

---

## 6.4 CAMS Global Fire Assimilation System (GFAS) — Daily Emissions

**URL:** https://ads.atmosphere.copernicus.eu/datasets/cams-global-fire-emissions-gfas  
**Maintained by:** ECMWF / CAMS  
**License:** Copernicus License v1.2

*(Full description in [04_climate_weather.md](04_climate_weather.md))*

Provides **daily** global fire emissions at 0.1° by assimilating MODIS Fire Radiative Power. Key advantage over GFED is daily temporal resolution. Used in CAMS atmospheric chemistry forecasts and reanalysis.

### Access via ADS API
```python
import cdsapi
c = cdsapi.Client(url='https://ads.atmosphere.copernicus.eu/api/v2', key='YOUR_KEY')
c.retrieve(
    'cams-global-fire-emissions-gfas',
    {
        'variable': ['co2_wildfire', 'wildfire_radiative_power'],
        'date': '2023-08-01/2023-08-31',
        'format': 'netcdf'
    },
    'gfas_august_2023.nc'
)
```

---

## 6.5 Global Fire Emissions from Indonesia / SIPONGI (KLHK)

**URL:** https://sipongi.menlhk.go.id/hotspot/lihat_hotspot  
**Maintained by:** Indonesian Ministry of Environment and Forestry (KLHK)  
**License:** Open (Indonesian Government)

### Description
Indonesia is responsible for the largest episodic fire emissions in the world, driven by tropical peatland burning in Sumatra and Kalimantan. SIPONGI integrates MODIS/VIIRS fire detection data and provides:
- Daily hotspot maps
- Province-level statistics
- Historical archive

### Relevance for Emissions Trend Analysis
Indonesia's 2015 El Niño fire season emitted more CO₂ than Germany's entire annual fossil fuel emissions. Any global emissions trend analysis must include this source.

---

## 6.6 FINN — Fire INventory from NCAR

**URL:** https://www2.acom.ucar.edu/modeling/finn-fire-inventory-ncar  
**Maintained by:** National Center for Atmospheric Research (NCAR)  
**License:** Open for research

### Description
The Fire INventory from NCAR (FINN) provides **daily, global, 1-km resolution** fire emission estimates by combining MODIS active fire detections with land cover maps and emission factors. Unlike GFED (which uses burned area), FINN uses fire radiative power and active fire counts, providing near-real-time emissions estimates with ~1-day latency.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 2002–present |
| Spatial resolution | 1 km |
| Temporal resolution | Daily |
| Spatial coverage | Global |
| Format | ASCII text, NetCDF |

### Variables
- CO₂, CO, CH₄, NOx, VOCs, black carbon, organic carbon
- Segregated by vegetation type (tropical forest, boreal, grassland, peat)

### Strengths
- Daily resolution (unlike monthly GFED)
- Near-real-time availability
- 1-km resolution captures fire spatial heterogeneity
- Used extensively in atmospheric chemistry models (WRF-Chem, GEOS-Chem)

### Limitations
- Less accurate than GFED for long-term trend analysis (GFED's burned area algorithm is more mature)
- Active fire detections miss fires under cloud cover more than burned area products
