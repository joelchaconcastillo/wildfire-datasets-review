# Category 5 — Regional / National Fire Databases

National and regional fire agencies maintain authoritative records of fire occurrence within their jurisdictions. These databases typically provide detailed attributes (cause, suppression resources, perimeters) that satellite products lack, but they are geographically bounded. They are invaluable for regional model development and for validating global satellite-based products.

---

## 5.1 FPA FOD — Fire Program Analysis Fire-Occurrence Database (USA)

**URL:** https://www.fs.usda.gov/rds/archive/Catalog/RDS-2013-0009  
**Maintained by:** USDA Forest Service  
**License:** Public Domain

*(Full description in [01_processed_ml_ready.md](01_processed_ml_ready.md#11-fpa-fod--fire-program-analysis-fire-occurrence-database-usa))*

The FPA FOD is the unified US fire occurrence database integrating records from Forest Service, DOI agencies, and state systems. Version 6 covers 1992–2020 with ~1.88 million records.

---

## 5.2 WFIGS — Wildland Fire Incident Geospatial System (USA, Current)

**URL:** https://data-nifc.opendata.arcgis.com/  
**Maintained by:** National Interagency Fire Center (NIFC)  
**License:** Public Domain

### Description
WFIGS is the current operational system replacing legacy IRWIN/ICS databases. It maintains **real-time and historical** wildland fire incidents with GIS perimeters updated daily during fire season. Available via ArcGIS REST API (see [03_live_api_endpoints.md](03_live_api_endpoints.md)).

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 2019–present (operational) |
| Spatial coverage | USA (all federal + reporting state/local fires) |
| Format | GeoJSON, Shapefile (REST API) |
| Spatial resolution | Mapped perimeters (polygon), point of origin |
| Variables | Incident name, agency, discovery date, containment, size, cause |

### Unique Value
Provides **current-season perimeters updated within hours**, making it suitable for near-real-time fire perimeter tracking.

---

## 5.3 Canadian National Fire Database (CNFDB)

**URL:** https://cwfis.cfs.nrcan.gc.ca/ha/nfdb  
**Maintained by:** Natural Resources Canada, Canadian Forest Service  
**License:** Open Government Licence – Canada

### Description
The CNFDB is the authoritative, comprehensive database of Canadian wildfire occurrences compiled from provincial, territorial, and Parks Canada fire records. It contains over **500,000 fire records** from 1959 to the most recent completed fire season. Records include fire location (point), fire size (ha), cause (lightning/person), general fuel type, and administrative jurisdiction.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Records | >500,000 fires |
| Period | 1959–present (updated annually) |
| Spatial coverage | Canada (all provinces and territories) |
| Format | Shapefile, geodatabase (.gdb) |
| Variables | Location, size (ha), cause, start date, agency, province/territory |

### Strengths
- Longest national record after USA
- Includes lightning-caused vs. human-caused classification
- Spatially representative of boreal forest fire regime

### Limitations
- No perimeters (point data only for older records)
- Some provinces have better coverage than others

---

## 5.4 EFFIS Fire Database (Europe + Mediterranean)

**URL:** https://effis.jrc.ec.europa.eu/applications/fire-history  
**Maintained by:** EC Joint Research Centre (JRC)  
**License:** Open (Copernicus License)

### Description
The EFFIS database integrates fire occurrence records from national fire agencies of EU member states and 43 countries in the broader European and Mediterranean region. Historical fire perimeters are available from the 1980s in some countries, with systematic pan-European coverage from 2000 onwards. The database distinguishes between confirmed fire perimeters (from official sources) and perimeters derived from MODIS satellite data.

### Geographic Scope
EU Member States + Western Balkans + Turkey + North Africa (Maghreb) + Middle East

*See deep-dive in [`../top_sources/effis/README.md`](../top_sources/effis/README.md)*

---

## 5.5 Australian Fire Incident Reporting System (AFIRS) / AFAC Data

**URL:** https://www.afac.com.au/initiative/narac/data  
**Maintained by:** Australasian Fire and Emergency Service Authorities Council (AFAC)  
**License:** Restricted (application required); aggregated statistics openly available

### Description
AFAC coordinates fire data collection from Australia's eight state and territory fire agencies. The National Resource Sharing Centre compiles national fire occurrence statistics. State-level agencies (NSW RFS, VIC CFA/DELWP, QLD QFES, WA DFES) maintain their own spatial fire history databases, most of which are publicly accessible.

### State-level Open Portals
| State | Data Portal | URL |
|-------|------------|-----|
| New South Wales | NSW RFS Fire History | https://www.rfs.nsw.gov.au/fire-information/fires-near-me |
| Victoria | DELWP Fire History | https://www.ffm.vic.gov.au/research-and-data/fire-history |
| Queensland | QFES Open Data | https://www.data.qld.gov.au |
| Western Australia | DFES | https://www.dfes.wa.gov.au |

### Relevance
Australia is one of the most fire-prone countries globally. The 2019–20 Black Summer fires (>18 million ha burned) make Australian data critical for extreme fire event analysis.

---

## 5.6 INPE BDQueimadas — Brazilian Fire Database

**URL:** https://queimadas.dgi.inpe.br/queimadas/bdqueimadas  
**Maintained by:** INPE (Instituto Nacional de Pesquisas Espaciais)  
**License:** Open (Brazilian Government open data)

### Description
Brazil's most comprehensive fire monitoring database, integrating MODIS, VIIRS, AQUA-TERRA, and other satellite sources. INPE has monitored deforestation and fire in the Amazon since the 1980s. BDQueimadas contains daily active fire detections from multiple sensors, filterable by biome (Amazon, Cerrado, Caatinga, Pantanal, Atlantic Forest, Pampa).

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 1998–present |
| Spatial coverage | Brazil (full national coverage) |
| Format | CSV download, REST API |
| Variables | Date/time, lat/lon, biome, state, municipality, satellite |
| API | Yes (see [03_live_api_endpoints.md](03_live_api_endpoints.md)) |

### Relevance
The Amazon and Cerrado are the most fire-active biomes globally by count. INPE data is the only official source with biome-level disaggregation.

---

## 5.7 South African National Parks (SANParks) Fire Records

**URL:** https://www.sanparks.org/about/scientific-services  
**Maintained by:** SANParks Scientific Services  
**License:** Open for research (application required)

### Description
SANParks maintains detailed fire history records for protected areas including Kruger National Park (fire records since 1941), Table Mountain National Park, and others. The Kruger fire atlas is one of the longest continuous fire records in the world and is used to study savanna fire regime dynamics.

---

## 5.8 Russian Federal Forestry Agency (Rosleshoz) Fire Data

**URL:** https://aviales.ru/popup.aspx?show=3  
**Data Portal:** https://public.aviales.ru/  
**Maintained by:** Federal Agency for Forestry (Rosleshoz), Russia  
**License:** Open (Russian Government)

### Description
Russia is the country with the largest annual burned area globally (especially Siberian boreal forest). Rosleshoz maintains the official national fire database and the Aviation Forest Protection service (Avialesookhrana) provides a public fire monitoring portal with active fire maps derived from MODIS and VIIRS data.

### Relevance
Siberian fires have increased dramatically in the 21st century and have global climate implications through carbon emissions and albedo change. Essential for global trend analysis.

---

## 5.9 Global Wildfire Information System (GWIS) — Country Profiles

**URL:** https://gwis.jrc.ec.europa.eu/apps/country.profile/  
**Maintained by:** EC JRC  
**License:** Open (Copernicus License)

### Description
GWIS Country Profiles provide standardised national-level statistics for every country, including:
- Annual burned area (from MODIS MCD64A1)
- Number of fire detections (VIIRS/MODIS)
- Fire-prone months
- Vegetation type breakdown of burned area
- CO and CO₂ emissions

### Access
Downloadable CSV files and an interactive web dashboard. API access available (see [03_live_api_endpoints.md](03_live_api_endpoints.md)).

---

## 5.10 CALFIRE Incident Archive (California, USA)

**URL:** https://www.fire.ca.gov/incidents/  
**Maintained by:** California Department of Forestry and Fire Protection (CAL FIRE)  
**License:** Public Domain (California Government)

### Description
California is one of the most studied fire regions globally due to its increasing fire frequency and severity. CAL FIRE's incident archive covers all fires on state-responsibility lands and provides:
- Fire name, start/end dates
- Acres burned, structures destroyed
- County and coordinates
- Links to full incident reports

### Historical Fire Perimeters
California Fire Perimeters (all years) are available from FRAP (Fire and Resource Assessment Program) at 1 km minimum mapping unit:  
https://www.fire.ca.gov/what-we-do/fire-resource-assessment-program/fire-perimeters
