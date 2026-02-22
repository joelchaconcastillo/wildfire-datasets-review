# Paper Outline: "Wildfire on Earth: Trends, Patterns, and Relevant Factors"

This document outlines a proposed academic paper leveraging the data sources catalogued in this repository. It is intended for submission to a journal such as *Earth System Science Data*, *Remote Sensing of Environment*, *Nature Climate Change*, or *Global Change Biology*.

---

## Proposed Title

**"Wildfire on Earth: Global Trends, Regional Patterns, and Climatic Drivers over the Satellite Era (2000–2023)"**

*Alternative working title:* "Two Decades of Global Fire Dynamics: A Multi-Source Earth Observation Analysis"

---

## Motivation

The global fire regime is changing. Fire seasons are lengthening, high-severity events are increasing, and previously fire-rare ecosystems (boreal forests, Mediterranean scrub, temperate rainforests) are experiencing unprecedented fire activity. Understanding *where*, *when*, *how much*, and *why* fires are changing is a prerequisite for effective fire management, climate adaptation, and early-warning system development. 

This paper provides a comprehensive multi-source synthesis of global wildfire trends from 2000–2023, integrating:
- Satellite-derived active fire detections (NASA FIRMS VIIRS/MODIS)
- Burned area products (GFED 4.1s, MCD64A1, ESA Fire_cci)
- Fire emissions (GFED, GFAS)
- Climate drivers (ERA5, FWI system, PDSI, SPEI, VPD)
- Regional ground-truth records (EFFIS, MTBS, FPA FOD, CNFDB)

---

## Proposed Journal Targets

| Journal | Impact Factor | Scope |
|---------|--------------|-------|
| *Nature Climate Change* | ~35 | High-impact climate-fire nexus |
| *Remote Sensing of Environment* | ~13 | Satellite methodology + trend analysis |
| *Global Change Biology* | ~10 | Ecosystem-level fire impacts |
| *Earth System Science Data* | ~11 | Data synthesis focus |
| *Environmental Research Letters* | ~6 | Accessible, broad scope |
| *International Journal of Wildland Fire* | ~3 | Wildfire specialist audience |

---

## Paper Structure

### Abstract (250 words)
- Problem: accelerating fire trends, uneven geographic picture
- Approach: multi-source satellite + climate analysis 2000–2023
- Key findings (to be determined by analysis)
- Implications: fire prediction, climate policy, management

---

### 1. Introduction

1.1 The Global Fire Regime and Its Importance  
- Fire as an Earth system process: ~2 Pg C/year emitted
- Fire–climate feedback loops
- Socioeconomic impacts: health (smoke, PM2.5), infrastructure, ecosystem services

1.2 Motivation for a Global Synthesis  
- Previous studies mostly regional or shorter time series
- New satellite record length (VIIRS 2012, MODIS 2000) enables 20+ year trend detection
- Climate extremes (2019–20 Australia, 2021 Siberia, 2022 Europe) demand updated global picture

1.3 Research Questions  
1. What are the global and regional trends in fire occurrence, burned area, and emissions from 2000 to 2023?
2. How do trends differ across biomes (tropical forest, savanna, boreal, Mediterranean, temperate)?
3. Which climatic drivers (temperature, drought, VPD, FWI) best explain interannual variability in fire activity?
4. What is the relative contribution of climate vs. land use change to observed fire trends?
5. Are fire seasons lengthening globally? What drives regional differences in fire seasonality?

---

### 2. Data and Methods

2.1 Fire Occurrence Data  
- NASA FIRMS VIIRS 375 m (2012–2023) — primary active fire dataset
- NASA FIRMS MODIS 1 km (2000–2023) — extends record back to 2000
- Methodology: grid aggregation to 0.1° × 0.1° monthly cells; confidence filtering (nominal+high)

2.2 Burned Area Data  
- GFED 4.1s (1997–2020) — primary for long-term trend analysis
- MCD64A1 v061 (2000–2023) — extends to present
- ESA Fire_cci v5.1 (2001–2020) — cross-validation

2.3 Fire Emissions Data  
- GFED 4.1s: monthly CO₂, CO, black carbon by biome
- GFAS: daily fire radiative power and emissions

2.4 Climate Driver Data  
| Variable | Source | Spatial Res. | Temporal Res. |
|----------|--------|--------------|----------------|
| Temperature (2m) | ERA5 | 0.25° | Monthly mean |
| Precipitation | ERA5 | 0.25° | Monthly total |
| VPD | ERA5 (derived) | 0.25° | Monthly mean |
| FWI | CDS CEMS-Fire | 0.1° | Monthly mean of daily max |
| PDSI | CRU TS 4.07 | 0.5° | Monthly |
| SPEI-3, SPEI-6 | CSIC SPEI DB | 0.5° | Monthly |
| ENSO (Niño 3.4 index) | NOAA PSL | Global | Monthly |

2.5 Land Cover and Biome Classification  
- ESA WorldCover 2021 (10 m) → resampled to 0.1° for biome fractions
- MODIS MCD12Q1 Annual Land Cover (500 m, 2001–2022) → temporal consistency
- GFED biome classification (6 classes) for consistency with emissions literature

2.6 Regional Reference Datasets  
- EFFIS: European fire statistics (2000–2023)
- MTBS: US burn severity trends (1984–2022)
- FPA FOD: US fire occurrence (1992–2020)
- CNFDB: Canadian fire occurrence (1959–2022)

2.7 Statistical Methods  
- **Trend detection:** Mann-Kendall non-parametric trend test + Theil-Sen slope estimator
- **Attribution:** Partial correlation, multiple regression, random forest feature importance
- **Seasonality analysis:** Fast Fourier Transform (FFT) of monthly fire time series; centroid-of-season metric
- **Teleconnection analysis:** Cross-correlation ENSO index vs. regional burned area (lag 0–12 months)
- **Spatial clustering:** Moran's I for spatial autocorrelation; DBSCAN for fire hotspot identification

---

### 3. Results

3.1 Global Burned Area Trends (2000–2023)  
- Total global burned area trend (Mha/year)
- Regional breakdown: which regions increasing, which decreasing?
- Comparison of GFED, MCD64A1, Fire_cci — inter-product agreement
- Specific focus: boreal increase, African savanna decrease, Australian/Mediterranean variability

3.2 Fire Frequency and Intensity Trends  
- VIIRS active fire detection trends (2012–2023) — detections/year by region
- Fire Radiative Power trends (GFAS) — proxy for fire intensity
- Number of "extreme fire events" (FRP > threshold) per year

3.3 Fire Emissions Trends  
- Global fire CO₂ emissions: trend and ENSO signal
- Black carbon and organic carbon trends (air quality implications)
- Peatland fire contribution (Indonesia, Russia) — high variability events

3.4 Fire Seasonality Changes  
- Length of fire season by region (first and last fire detection dates)
- Shift in peak fire month (has fire season moved earlier in calendar year?)
- Double-peaked seasonality in some regions (Mediterranean: spring + summer)

3.5 Climatic Drivers — Attribution Analysis  
- Correlation analysis: FWI vs. burned area by biome
- VPD anomaly as a predictor of high-fire years
- ENSO lagged response: 3–6 month lag between Niño 3.4 index and fire activity in tropical regions
- Temperature trend contribution to VPD increase → fire danger increase

3.6 Land Use Change vs. Climate Attribution  
- Deforestation-driven fire trends in Amazon (INPE data)
- Land abandonment and fire in Europe (abandoned agricultural land → fuel accumulation)
- Fire suppression effect in North America (fuel accumulation → larger fires when ignition occurs)

---

### 4. Discussion

4.1 Key Findings in Global Context  
- Reconcile the "global burned area is declining" finding (Andela et al., 2017) with the "fire is getting worse" narrative
- Resolution: area declining mainly in African savanna (land use change); severity and frequency increasing in forests
- The "global" number masks divergent regional trends

4.2 Climate Change Amplification  
- Quantify the fraction of trend attributable to anthropogenic warming (following Abatzoglou & Williams approach)
- Implications for future scenarios (CMIP6 + SSP projections)

4.3 Implications for Fire Prediction Models  
- Which drivers are most predictive at seasonal timescale?
- Recommended feature set for a global daily prediction model
- Challenges: data gaps in tropical regions (cloud cover), rapidly changing land use

4.4 Limitations  
- Satellite detection limitations (cloud cover, detection probability by fire size)
- GFED ends at 2020; need GFED 5 for complete analysis to 2023
- Regional databases not globally harmonised

---

### 5. Conclusions

- Summary of major global trends
- The divergent story: Africa declining, forests increasing
- Climate as dominant driver in the satellite era
- Call for improved global fire monitoring (more frequent revisit, better detection under smoke)
- Data infrastructure needs for next-generation fire prediction

---

### References (Key Papers to Include)

1. Andela, N., et al. (2017). A human-driven decline in global burned area. *Science*, 356, 1356–1362.
2. van der Werf, G.R., et al. (2017). Global fire emissions estimates during 1997–2016. *Earth Syst. Sci. Data*, 9, 697–720.
3. Abatzoglou, J.T., & Williams, A.P. (2016). Impact of anthropogenic climate change on wildfire across western US forests. *PNAS*, 113, 11770–11775.
4. Jones, M.W., et al. (2022). Global and regional trends and drivers of fire under climate change. *Reviews of Geophysics*, 60.
5. Westerling, A.L. (2016). Increasing western US forest wildfire activity. *Phil. Trans. R. Soc. B*, 371.
6. Bowman, D.M.J.S., et al. (2020). The human dimensions of fire regimes on Earth. *Journal of Biogeography*, 47(12).
7. Giglio, L., et al. (2018). The Collection 6 MODIS burned area mapping algorithm and product. *Remote Sensing of Environment*, 217, 72–85.
8. Vitolo, C., et al. (2019). ERA5-based global meteorological wildfire danger maps. *Scientific Data*, 6, 216.
9. Andela, N., et al. (2019). The Global Fire Atlas of individual fire size, duration, speed and direction. *Earth Syst. Sci. Data*, 11, 529–552.

---

## Data Availability Statement (Template)

```
All data used in this study are publicly available:
- NASA FIRMS VIIRS active fire detections: https://firms.modaps.eosdis.nasa.gov/
- GFED 4.1s burned area and emissions: https://www.globalfiredata.org/
- ERA5 reanalysis and FWI: https://cds.climate.copernicus.eu/
- MODIS MCD64A1 burned area: https://lpdaac.usgs.gov/products/mcd64a1v061/
- ESA Fire_cci: https://climate.esa.int/en/projects/fire/
- MTBS fire severity: https://www.mtbs.gov/
- EFFIS fire statistics: https://effis.jrc.ec.europa.eu/
Analysis code is available at: [GitHub URL to be added upon publication]
```

---

## Suggested Figures

| Figure # | Content | Data Source |
|----------|---------|-------------|
| 1 | Global map: mean annual burned area fraction (2000–2022) | MCD64A1 / GFED |
| 2 | Time series: global burned area by biome (1997–2022) | GFED 4.1s |
| 3 | Trend map: Mann-Kendall trend in annual fire detections (2000–2023) | FIRMS MODIS |
| 4 | Regional time series: 14 GFED regions, burned area anomalies | GFED |
| 5 | Fire seasonality: circular diagrams showing peak fire month by biome | FIRMS VIIRS |
| 6 | Correlation map: FWI vs. burned area (monthly) | CDS + MCD64A1 |
| 7 | ENSO teleconnection: correlation map (Niño 3.4 vs. fire detections, 3-month lag) | FIRMS + NOAA |
| 8 | Fire season length trend: days per year with fire above threshold (1984–2022) | MTBS (USA) + FIRMS |
| 9 | VPD trend vs. fire trend (scatter by region) | ERA5 + GFED |
| 10 | Case study: 2019–20 Australian fires; 2021 Siberia; 2022 Europe | FIRMS + EFFIS |
