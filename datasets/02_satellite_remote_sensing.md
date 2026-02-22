# Category 2 — Satellite / Remote Sensing Fire Products

Satellite-derived products are the backbone of global fire monitoring. They provide spatially and temporally consistent observations that no ground network can match. This category includes active fire detection products, burned area products, and fire radiative power (FRP) time series.

---

## 2.1 MODIS Active Fire Product — MOD14 / MYD14 (Terra & Aqua)

**URL:** https://modis.gsfc.nasa.gov/data/dataprod/mod14.php  
**Data Access:** https://earthdata.nasa.gov (NASA Earthdata)  
**Maintained by:** NASA MODIS Land Science Team  
**License:** Public Domain

### Description
The MODIS active fire product uses the thermal anomaly algorithm (Giglio et al.) applied to MODIS 4 µm and 11 µm channels to detect active fires at **1 km resolution**. Terra (MOD14) passes at ~10:30 local time; Aqua (MYD14) at ~13:30 — providing two independent daily observations. Available since March 2000 (Terra) and July 2002 (Aqua). Collection 6.1 is the current standard.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | March 2000–present (Terra); July 2002–present (Aqua) |
| Spatial coverage | Global |
| Format | HDF4, HDF-EOS2 |
| Spatial resolution | 1 km |
| Temporal resolution | Up to 4 observations/day (Terra + Aqua) |
| Variables | Fire mask, FRP, brightness temperature (21, 31), confidence, view angle |

### Strengths
- 23+ year consistent global record
- Well-validated algorithm (Giglio et al., 2016)
- Used as the gold standard for global fire climatology

### Limitations
- Cloud cover and smoke can occlude detections
- 1 km resolution misses small fires

---

## 2.2 VIIRS Active Fire Product — VNP14 / VJ114 (SNPP & NOAA-20)

**URL:** https://viirsland.gsfc.nasa.gov/Products/NASA/Fire.html  
**Data Access:** https://firms.modaps.eosdis.nasa.gov  
**Maintained by:** NASA  
**License:** Public Domain

### Description
The Visible Infrared Imaging Radiometer Suite (VIIRS) active fire product at **375 m resolution** is the highest-resolution global operational fire detection product from satellite. SNPP (VNP14) launched in 2011; NOAA-20 (VJ114) launched in 2018 as a backup and overlap. Both provide near-real-time (3-hour latency) data through NASA FIRMS.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | SNPP: January 2012–present; NOAA-20: January 2018–present |
| Spatial coverage | Global |
| Format | CSV (FIRMS download), HDF5 (EarthData) |
| Spatial resolution | 375 m (I-band) |
| Temporal resolution | Daily; near-real-time within 3 hours |
| Variables | Lat, lon, brightness, FRP, confidence, day/night flag |

### Strengths
- Best combination of spatial resolution and global coverage
- 375 m detection catches smaller fires than MODIS
- Near-real-time availability → suitable for operational forecast systems

### Limitations
- Record only from 2012 (shorter than MODIS)
- Still limited by cloud cover and thick smoke

---

## 2.3 MODIS Burned Area — MCD64A1

**URL:** https://lpdaac.usgs.gov/products/mcd64a1v061/  
**Maintained by:** NASA LP DAAC  
**License:** Public Domain

### Description
The MODIS Burned Area product (MCD64A1) provides global monthly 500-m burned area maps derived by the spectral, temporal and spatial contextual algorithm (Giglio et al., 2018). The algorithm detects burn scars by examining MODIS reflectance changes and active fire data. Each pixel is assigned a burn date (Julian day) or labelled as unburned/water/cloud.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | November 2000–present |
| Spatial coverage | Global |
| Format | HDF4, Cloud Optimised GeoTIFF (via AppEEARS) |
| Spatial resolution | 500 m |
| Temporal resolution | Monthly |
| Variables | Burn date, burn date uncertainty, QA flags |

### Strengths
- Long record (2000–present) at moderate resolution
- Used by GFED for global fire emissions accounting

---

## 2.4 ESA Climate Change Initiative Burned Area — Fire_cci

**URL:** https://climate.esa.int/en/projects/fire/  
**Data Access:** https://catalogue.ceda.ac.uk/uuid/58f00d8814064b79a0c49662ad3af537  
**Maintained by:** European Space Agency (ESA) Climate Change Initiative  
**License:** Open (CC BY 4.0)

### Description
ESA's Fire_cci product provides long-term, satellite-derived burned area at **250 m resolution** using MODIS (2001–2020). The v5.1 release covers 2001–2020 globally. It is one of the ESA Essential Climate Variables (ECV) and is particularly important for climate model validation.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 2001–2020 (v5.1) |
| Spatial coverage | Global |
| Format | NetCDF-4 |
| Spatial resolution | 250 m |
| Temporal resolution | Monthly |
| Variables | Burned area, number of patches, standard error, land cover class at time of burn |

### Strengths
- Peer-reviewed algorithm (Chuvieco et al., 2018)
- Includes land cover information at time of burn (fuel type)

---

## 2.5 Sentinel-2 Multispectral Instrument (MSI)

**URL:** https://sentinel.esa.int/web/sentinel/missions/sentinel-2  
**Data Access:** https://scihub.copernicus.eu/ | https://earth.esa.int/eogateway/tools/sentinel-hub  
**Maintained by:** European Space Agency (ESA) / Copernicus  
**License:** Free, open access (Copernicus Open Access Hub)

### Description
Sentinel-2 is a constellation of two satellites (2A launched 2015, 2B launched 2017) providing **10 m resolution** multispectral imagery every 5 days at the equator (2–3 days at mid-latitudes). While not a dedicated fire product, Sentinel-2 Level-2A surface reflectance and Short-Wave Infrared (SWIR) bands 8A and 12 are the standard for high-resolution burn scar mapping. The Copernicus Emergency Management Service (CEMS) uses Sentinel-2 for post-fire damage assessments.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | June 2015–present |
| Spatial coverage | Global (land) |
| Format | SAFE (JP2000), GeoTIFF via APIs |
| Spatial resolution | 10–60 m |
| Temporal resolution | 5 days (2A+2B constellation) |
| Fire-relevant bands | B8A (NIR, 865 nm), B11 (SWIR, 1610 nm), B12 (SWIR, 2190 nm) |

### Use for Fire Detection
The differenced Normalized Burn Ratio (dNBR = NBR_pre − NBR_post) computed from Sentinel-2 is the standard high-resolution burn severity index.

---

## 2.6 Landsat Collection 2 (USGS/NASA)

**URL:** https://www.usgs.gov/core-science-systems/nli/landsat  
**Data Access:** https://earthexplorer.usgs.gov/ | https://landsatlook.usgs.gov/  
**Maintained by:** USGS / NASA  
**License:** Public Domain

### Description
Landsat provides the **longest continuous Earth observation record from space** (since 1972, with radiometrically calibrated data from Landsat 4 onwards, 1982). Landsat 8 (2013) and Landsat 9 (2021) provide 30-m multispectral imagery every 16 days. MTBS (see 1.9) is built on Landsat. The USGS releases analysis-ready data (ARD) in CONUS tiles.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | 1972–present (scientifically useful from ~1984) |
| Spatial coverage | Global |
| Format | GeoTIFF |
| Spatial resolution | 30 m (multispectral), 15 m (panchromatic) |
| Temporal resolution | 16 days per sensor; 8 days with L8+L9 combination |
| Fire-relevant bands | Band 5 (NIR), Band 6 (SWIR-1), Band 7 (SWIR-2) |

---

## 2.7 GOES-16 / GOES-17 / GOES-18 Active Fire Detection (USA / Americas)

**URL:** https://www.goes.noaa.gov/  
**Data Access:** https://registry.opendata.aws/noaa-goes/  
**Maintained by:** NOAA  
**License:** Public Domain (US Government)

### Description
NOAA's Geostationary Operational Environmental Satellites provide **15-minute to 1-minute** fire detection over the Americas. The Fire Detection and Characterization product (FDCA/FDC) uses GOES-ABI channels 7 (3.9 µm) and 14 (11.2 µm). Unlike polar-orbit satellites, GOES provides continuous temporal monitoring of the full Western Hemisphere.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | GOES-16: December 2017–present |
| Spatial coverage | Western Hemisphere (Americas + adjacent oceans) |
| Format | NetCDF-4 (AWS S3 public bucket) |
| Spatial resolution | ~2 km at nadir |
| Temporal resolution | 1–15 minutes |
| Variables | Fire area, power, temperature, mask confidence |

### Strengths
- Sub-hourly temporal resolution is unique among global fire products
- Real-time data available on AWS S3 with <15-minute latency

---

## 2.8 Himawari-8 / Himawari-9 Active Fire Detection (Asia-Pacific)

**URL:** https://www.jma.go.jp/jma/jma-eng/satellite/  
**Data Access:** https://www.eorc.jaxa.jp/ptree/  
**Maintained by:** Japan Meteorological Agency (JMA) / JAXA  
**License:** Open for research (registration required)

### Description
The Himawari-8 and 9 geostationary satellites (JMA) cover the **Asia-Pacific** region with 10-minute scan frequency, analogous to GOES over the Americas. The JAXA P-Tree system distributes fire-related products including active fire detections and hotspot data, critical for monitoring fire-prone regions like Southeast Asia, Australia, and East Siberia.

### Key Attributes
| Attribute | Detail |
|-----------|--------|
| Period | July 2015–present (Himawari-8) |
| Spatial coverage | Asia-Pacific, 60°E–160°W, 80°N–80°S |
| Format | NetCDF, HSD format |
| Spatial resolution | 2 km |
| Temporal resolution | 10 minutes |
