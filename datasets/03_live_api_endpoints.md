# Category 3 — Live APIs & Real-time Data Endpoints

These services expose wildfire data via programmatic interfaces (REST APIs, OGC Web Services, WebSockets, or bulk streaming). They are essential for building automated ingestion pipelines, near-real-time dashboards, and operational prediction systems.

---

## 3.1 NASA FIRMS API — Fire Information for Resource Management System

**URL:** https://firms.modaps.eosdis.nasa.gov/api/  
**Maintained by:** NASA Earth Science Data and Information System (ESDIS)  
**License:** Public Domain  
**Authentication:** Free MAP_KEY registration at https://firms.modaps.eosdis.nasa.gov/api/map_key/

### Description
The primary global API for active fire data. FIRMS exposes MODIS (1 km) and VIIRS SNPP/NOAA-20 (375 m) active fire detections through a straightforward HTTP REST interface. Queries can be parameterised by:
- **Area of Interest**: bounding box or country code
- **Temporal range**: last 24 h, 48 h, 7 days, or date range
- **Product**: `MODIS_NRT`, `VIIRS_SNPP_NRT`, `VIIRS_NOAA20_NRT`, `MODIS_SP`, `VIIRS_SNPP_SP`

### Endpoints

```
# Active fires (NRT) - CSV
GET https://firms.modaps.eosdis.nasa.gov/api/area/csv/{MAP_KEY}/{product}/{bbox}/{days}

# Active fires by country
GET https://firms.modaps.eosdis.nasa.gov/api/country/csv/{MAP_KEY}/{product}/{country}/{days}

# Transactions (rate-limit check)
GET https://firms.modaps.eosdis.nasa.gov/api/area/csv/{MAP_KEY}/VIIRS_SNPP_NRT/world/1
```

### Response Fields (VIIRS)
`latitude, longitude, bright_ti4, scan, track, acq_date, acq_time, satellite, instrument, confidence, version, bright_ti5, frp, daynight`

### Rate Limits
- 5,000 transactions/day per MAP_KEY (each area/country/day call = 1 transaction)
- Unlimited bulk download via separate archive endpoint

### Strengths
- Only global, freely accessible active fire REST API with near-real-time data (<3 h latency)
- Supports GeoJSON, CSV, and KML outputs
- Stable, well-documented, used in hundreds of operational systems

---

## 3.2 Copernicus Climate Data Store (CDS) API

**URL:** https://cds.climate.copernicus.eu/api-how-to  
**Maintained by:** ECMWF / Copernicus Climate Change Service (C3S)  
**License:** Copernicus License (free for any use including commercial)  
**Authentication:** Free account at https://cds.climate.copernicus.eu/

### Description
The CDS API (`cdsapi` Python package) provides programmatic access to the full Copernicus climate and fire weather catalogue. Key datasets for fire prediction:

- **ERA5 Reanalysis** (`reanalysis-era5-single-levels`) — temperature, humidity, wind, precipitation, 1979–present, hourly, 0.25°
- **ERA5-Land** (`reanalysis-era5-land`) — higher-resolution land surface variables, 0.1°
- **EFFIS FWI** — Fire Weather Index components (FFMC, DMC, DC, ISI, BUI, FWI) computed from ERA5
- **Fire Danger Indices** (`cems-fire-historical`) — Historical FWI, drought codes, fire season indicators

### Python Example

```python
import cdsapi
c = cdsapi.Client()
c.retrieve(
    'reanalysis-era5-single-levels',
    {
        'product_type': 'reanalysis',
        'variable': ['2m_temperature', 'total_precipitation',
                     '10m_u_component_of_wind', '10m_v_component_of_wind',
                     '2m_dewpoint_temperature'],
        'year': '2023', 'month': '08', 'day': ['01','02','03'],
        'time': ['06:00', '12:00', '18:00'],
        'format': 'netcdf'
    },
    'fire_weather_august_2023.nc'
)
```

### Key Products for Fire Prediction
| Product ID | Description | Resolution | Frequency |
|------------|-------------|------------|-----------|
| `reanalysis-era5-single-levels` | Atmospheric reanalysis | 0.25° | Hourly |
| `reanalysis-era5-land` | Land surface reanalysis | 0.1° | Hourly |
| `cems-fire-historical` | Fire Danger Indices (FWI) | 0.1° | Daily |
| `seasonal-original-single-levels` | Seasonal forecasts | 1° | Monthly |

*See deep-dive analysis in [`../top_sources/copernicus_cds/README.md`](../top_sources/copernicus_cds/README.md)*

---

## 3.3 Global Wildfire Information System (GWIS) API

**URL:** https://gwis.jrc.ec.europa.eu/  
**API endpoint:** https://gwis.jrc.ec.europa.eu/apps/country.profile/downloads  
**Maintained by:** EC Joint Research Centre (JRC) in partnership with Copernicus  
**License:** Open (Copernicus License)

### Description
GWIS is the global-scale complement to EFFIS. It integrates MODIS and VIIRS active fire data with climate and vegetation information to provide fire danger assessment at global and national scales. The web portal and GeoServer-based WMS/WFS endpoints allow query by country, region, and date.

### Available Data Layers
- Active fire detections (MODIS, VIIRS)
- Burned area (MODIS MCD64A1 monthly)
- Fire danger forecast (FWI, from ECMWF GEFF model)
- CO emissions (from GFAS — Global Fire Assimilation System)

### API Access
```
# OGC WMS endpoint (layers include active fires, burned area, FWI)
https://maps.gwis.jrc.ec.europa.eu/geoserver/gwis/wms?service=WMS&version=1.1.1
    &request=GetMap&layers=gwis:viirs_fires_24h&...

# Download country statistics (CSV)
https://gwis.jrc.ec.europa.eu/apps/country.profile/downloads
```

---

## 3.4 EFFIS Web Services (European Forest Fire Information System)

**URL:** https://effis.jrc.ec.europa.eu/  
**WMS/WFS:** https://maps.effis.emergency.copernicus.eu/  
**Maintained by:** EC Joint Research Centre (JRC)  
**License:** Open (Copernicus License)

### Description
EFFIS provides real-time fire monitoring and forecast for EU member states and 43 countries in the wider European region. It exposes OGC Web Map Service (WMS) and Web Feature Service (WFS) endpoints for:
- Current active fires (from MODIS and VIIRS)
- Burned area perimeters (updated weekly during fire season)
- Fire Weather Index (FWI) daily danger maps

### Endpoint Example
```
# Active fires WFS
https://maps.effis.emergency.copernicus.eu/geoserver/gwis/wfs?
  service=WFS&version=2.0.0&request=GetFeature
  &typeNames=gwis:viirs_fires_24h&outputFormat=application/json
```

*See deep-dive in [`../top_sources/effis/README.md`](../top_sources/effis/README.md)*

---

## 3.5 NOAA National Centers for Environmental Information (NCEI) API

**URL:** https://www.ncei.noaa.gov/cdo-web/api/v2/  
**Maintained by:** NOAA NCEI  
**License:** Public Domain  
**Authentication:** Free token at https://www.ncdc.noaa.gov/cdo-web/token

### Description
The NOAA Climate Data Online (CDO) API provides access to historical weather observations from thousands of global stations. For fire research, the most relevant datasets are:
- **GSOD** (Global Surface Summary of Day) — daily temperature, humidity, wind, precipitation
- **GHCN-Daily** — high-quality daily climate normals and observations

### Endpoint Example
```
GET https://www.ncei.noaa.gov/cdo-web/api/v2/data?
    datasetid=GSOD&stationid=GHCND:USW00094728
    &startdate=2023-08-01&enddate=2023-08-31
    &limit=1000&token=YOUR_TOKEN
```

### Relevance
Useful for obtaining local weather observations to validate or supplement reanalysis data (ERA5) in fire models.

---

## 3.6 NIFC — National Interagency Fire Center Data API (USA)

**URL:** https://data-nifc.opendata.arcgis.com/  
**Maintained by:** National Interagency Fire Center (NIFC)  
**License:** Public Domain

### Description
NIFC publishes the US national fire data through ArcGIS Online Open Data. The portal exposes GeoJSON/REST endpoints for:
- **Current large fires** (updated daily during fire season)
- **Historical fire perimeters** (WFIGS — Wildland Fire Incident GeoSpatial database)
- **Year-to-date statistics**

### Endpoint Examples
```
# Current wildfire perimeters (GeoJSON)
https://services3.arcgis.com/T4QMspbfLg3qTGWY/arcgis/rest/services/
  WFIGS_Interagency_Perimeters_Current/FeatureServer/0/query?where=1=1&outFields=*&f=geojson

# Historical fire occurrence (1 km resolution)
https://services3.arcgis.com/T4QMspbfLg3qTGWY/arcgis/rest/services/
  WFIGS_Interagency_Fire_Perimeters_History/FeatureServer/0/query
```

---

## 3.7 INPE BDQueimadas API (Brazil)

**URL:** https://queimadas.dgi.inpe.br/queimadas/bdqueimadas  
**API:** https://queimadas.dgi.inpe.br/api/focos/  
**Maintained by:** Instituto Nacional de Pesquisas Espaciais (INPE), Brazil  
**License:** Open (Brazilian Government open data)

### Description
Brazil's National Institute for Space Research (INPE) operates the most comprehensive fire monitoring network for South America, integrating MODIS, VIIRS, AQUA, and TERRA fire products. The BDQueimadas (Fire Database) API provides daily active fire detections for Brazil and surrounding countries.

### Endpoint Example
```
# Active fires in the last 48 hours in Brazil (GeoJSON)
GET https://queimadas.dgi.inpe.br/api/focos/?pais_id=33&num_dias_anterior=2

# By biome (e.g., Amazon = bioma_id=1)
GET https://queimadas.dgi.inpe.br/api/focos/?bioma_id=1&num_dias_anterior=7
```

### Key Value
Only official API dedicated to fire monitoring in the Amazon and Brazilian Cerrado, the most fire-critical ecosystems in South America.

---

## 3.8 NASA Earthdata CMR (Common Metadata Repository) API

**URL:** https://cmr.earthdata.nasa.gov/search/  
**Maintained by:** NASA ESDIS  
**License:** Public Domain

### Description
The CMR is NASA's unified search API for all Earth science datasets. It enables programmatic discovery and access to fire-relevant datasets including MODIS, VIIRS, LANDSAT, and GEDI granules.

### Endpoint Example
```
# Find VIIRS active fire granules
GET https://cmr.earthdata.nasa.gov/search/granules.json?
    short_name=VNP14IMGTDL_NRT&temporal[]=2024-08-01T00:00:00Z,2024-08-07T00:00:00Z
    &bounding_box=-130,30,-60,60
```

---

## 3.9 OpenStreetMap / Overpass API (Fire Stations, Infrastructure)

**URL:** https://overpass-api.de/  
**Maintained by:** OpenStreetMap Foundation  
**License:** ODbL

### Description
While not a fire data source per se, the Overpass API is useful for extracting **infrastructure data** relevant to fire spread modelling and risk assessment: road networks (for accessibility/suppression), urban/wildland interface boundaries, land use, and protected area boundaries. Can be integrated with fire spread models for risk mapping.

### Endpoint Example
```
# Extract fire stations within a bounding box
[out:json];
node["amenity"="fire_station"](40,-120,42,-118);
out body;
```
