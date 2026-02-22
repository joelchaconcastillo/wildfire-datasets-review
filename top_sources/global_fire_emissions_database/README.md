# Deep-Dive: Global Fire Emissions Database (GFED)

**Official Site:** https://www.globalfiredata.org/  
**Maintained by:** GFED Science Team — Guido van der Werf (VU Amsterdam) et al.  
**License:** Open for research and educational use (cite van der Werf et al., 2017)  
**Overall Rank:** 🥉 #3 for global daily wildfire prediction

---

## Overview

The Global Fire Emissions Database (GFED) is the gold standard for long-term global fire carbon and emissions accounting. It combines satellite-derived burned area with a process-based biogeochemical model (CASA-GFED) to estimate monthly fire emissions of greenhouse gases and aerosols at the global 0.25° grid scale.

GFED is essential for:
1. **Training data enrichment** — historical burned area as a predictor of future fire probability
2. **Climate trend analysis** — 23+ year time series of global fire carbon emissions by biome
3. **Model validation** — benchmark for fire-prone region identification
4. **Academic paper foundation** — most cited global fire emissions dataset (>4,000 citations)

---

## Database Versions

| Version | Period | Key Changes |
|---------|--------|-------------|
| GFED 1 | 1997–2001 | Initial MODIS-based release |
| GFED 2 | 1997–2006 | Improved emission factors |
| GFED 3 | 1997–2009 | First use of MODIS burned area product |
| GFED 4 | 1997–2015 | MODIS MCD64A1 burned area, updated CASA model |
| **GFED 4.1s** | **1997–2020** | **Small fires added (MODIS active fire sampling)** |
| GFED 5 (beta) | 1997–present | New BA algorithm, updated emission factors |

**Current standard: GFED 4.1s** — use this for any published analysis until GFED 5 is formally released.

---

## Data Structure

### File Format
All GFED data is provided as **HDF5** files, one per year, with monthly layers.

```
GFED4.1s_2020.hdf5
├── burned_area/
│   ├── 01/   (January)
│   │   ├── burned_fraction   [720 × 1440 float32]  # fraction of 0.25° cell
│   └── ... (02 through 12)
├── emissions/
│   ├── 01/
│   │   ├── C     # Carbon emissions (gC m-2 month-1)
│   │   ├── CO2   # CO2 (gCO2 m-2 month-1)
│   │   ├── CO    # CO (gCO m-2 month-1)
│   │   ├── CH4   # CH4 (gCH4 m-2 month-1)
│   │   ├── NOx   # NOx (gNO m-2 month-1)
│   │   ├── BC    # Black carbon
│   │   └── OC    # Organic carbon
│   └── ... (02 through 12)
├── ancillary/
│   ├── grid_cell_area    [720 × 1440]   # m2 per cell
│   ├── basis_regions     [720 × 1440]   # 14 GFED regions (1–14)
│   └── biome             [720 × 1440]   # 6 biome classes
└── README
```

### Spatial Grid
- **Resolution:** 0.25° × 0.25° (approximately 27.75 km at equator)
- **Dimensions:** 720 rows × 1440 columns
- **Coverage:** Global (−90 to +90 latitude, −180 to +180 longitude)
- **Origin:** Upper-left corner at (90°N, 180°W)

### GFED Basis Regions (14 global regions)
| ID | Region | ID | Region |
|----|--------|-----|--------|
| 1 | BONA (Boreal North America) | 8 | CEAM (Central America) |
| 2 | TENA (Temperate North America) | 9 | NHSA (Northern Hemisphere South America) |
| 3 | CEAM (Central America) | 10 | SHSA (Southern Hemisphere South America) |
| 4 | NHSA | 11 | EURO (Europe) |
| 5 | SHSA | 12 | MIDE (Middle East) |
| 6 | EURO | 13 | NHAF (Northern Hemisphere Africa) |
| 7 | MIDE | 14 | SHAF (Southern Hemisphere Africa) |

*Full 14-region list at: https://www.globalfiredata.org/files/GFED4_Emission_Factors.txt*

### Biome Classes
| Code | Biome |
|------|-------|
| 1 | Tropical forests |
| 2 | Extratropical forests |
| 3 | Savanna, grasslands, and shrublands |
| 4 | Agricultural waste burning |
| 5 | Peatlands |
| 6 | Deforestation and degradation |

---

## Key Statistics from GFED 4.1s (1997–2020)

### Global Annual Fire Carbon Emissions
- **Average:** ~2.1 Pg C per year (range: 1.5 – 2.8 Pg C)
- **Largest emitters by region:** SHAF (Southern Hemisphere Africa) > TROP (Tropical forests) > BONA+BOAS (Boreal)
- **Trend:** Globally declining burned area from ~2000–2019 (Andela et al., 2017, *Science*), primarily due to African savanna fire reduction from land use change; partially offset by increasing extratropical forest fires

### Fire Emissions by Biome (approximate % of global total)
| Biome | % of Total C Emissions |
|-------|------------------------|
| Savanna, grasslands, shrublands | ~54% |
| Tropical forest (deforestation/degradation) | ~20% |
| Extratropical forests | ~12% |
| Peatlands | ~9% |
| Agricultural waste | ~5% |

### El Niño Signal
GFED clearly shows the El Niño amplification of fire:
- 1997–98 El Niño: ~3.0 Pg C (largest in record, driven by Indonesia peat)
- 2015 El Niño: ~2.8 Pg C
- La Niña years: typically 1.5–1.7 Pg C

---

## Python Access and Analysis

### Download GFED Data
```python
import requests
import os

# Download all years
base_url = "https://www.globalfiredata.org/data/GFED4.1s/"
years = range(1997, 2021)

os.makedirs("gfed_data", exist_ok=True)
for year in years:
    url = f"{base_url}GFED4.1s_{year}.hdf5"
    fname = f"gfed_data/GFED4.1s_{year}.hdf5"
    if not os.path.exists(fname):
        r = requests.get(url)
        with open(fname, 'wb') as f:
            f.write(r.content)
        print(f"Downloaded {year}")
```

### Read and Analyse Burned Area
```python
import h5py
import numpy as np

# Open GFED for one year
with h5py.File("gfed_data/GFED4.1s_2020.hdf5", "r") as f:
    # Grid cell area (m2)
    grid_area = f["ancillary/grid_cell_area"][:]
    
    # Biome classification
    biome = f["ancillary/biome"][:]
    
    # Annual burned area (sum across months)
    annual_ba = np.zeros((720, 1440))
    for month in range(1, 13):
        month_str = f"{month:02d}"
        ba_fraction = f[f"burned_area/{month_str}/burned_fraction"][:]
        annual_ba += ba_fraction
    
    # Convert to hectares
    annual_ba_ha = annual_ba * grid_area / 10000  # m2 to ha
    
    total_burned = annual_ba_ha.sum()
    print(f"Global burned area 2020: {total_burned/1e6:.1f} Mha")

# Typical output: ~350-550 Mha depending on year
```

### Compute Time Series by Region
```python
import h5py
import numpy as np
import pandas as pd

years = range(1997, 2021)
results = []

for year in years:
    with h5py.File(f"gfed_data/GFED4.1s_{year}.hdf5", "r") as f:
        grid_area = f["ancillary/grid_cell_area"][:]
        basis_regions = f["ancillary/basis_regions"][:]
        
        annual_co2 = np.zeros((720, 1440))
        for month in range(1, 13):
            m = f"{month:02d}"
            co2 = f[f"emissions/{m}/CO2"][:]
            annual_co2 += co2 * grid_area  # g CO2 total per cell
        
        # Global total (Pg CO2)
        global_co2_pg = annual_co2.sum() / 1e15
        results.append({'year': year, 'global_co2_pg': global_co2_pg})

df = pd.DataFrame(results)
print(df)
```

---

## Integration with Fire Prediction Models

### Use as Historical Burned Area Feature
Burned area climatology from GFED can be used as a **static or slowly-varying feature** in prediction models:
- **Mean burned fraction** per grid cell (1997–2020 average) → "historical fire propensity"
- **Coefficient of variation** of annual burned area → "fire variability"
- **Trend in burned area** (linear slope) → "changing fire regime"

### Temporal Alignment with VIIRS
Since GFED is monthly and VIIRS is daily, use GFED for:
- **Background burned area climatology** (annual mean/SD by month and location)
- **Biome-specific emission factors** for post-fire impact estimation
- **Validation** of model fire probability against observed burned area fractions

---

## GFED Data for Paper on Wildfire Trends

GFED is the ideal data source for the proposed paper "Wildfire on Earth: Trends, Patterns, and Relevant Factors" because:
1. **23-year global time series** enables trend detection
2. **Biome-level breakdown** enables ecological pattern analysis
3. **Multi-variable output** (BA, C, CO₂, CO, BC, OC) supports comprehensive factors analysis
4. **ENSO/climate teleconnection** analysis is well-supported by the interannual variability

### Suggested Analyses for the Paper
1. **Global burned area trend** (1997–2020, by biome)
2. **Regional fire emission trends** (14 GFED basis regions)
3. **Correlation with ENSO** (MEI vs. global fire C) — El Niño amplification
4. **Correlation with climate drivers** (PDSI, VPD, temperature anomaly)
5. **Biome shift analysis** — increasing extratropical vs. decreasing savanna fires

---

## Key Publications

1. **van der Werf et al. (2017)** — Current GFED4.1s reference:
   van der Werf, G.R., et al. (2017). Global fire emissions estimates during 1997–2016. *Earth System Science Data*, 9(2), 697–720. https://doi.org/10.5194/essd-9-697-2017

2. **Andela et al. (2017)** — Global burned area decline:
   Andela, N., et al. (2017). A human-driven decline in global burned area. *Science*, 356(6345), 1356–1362. https://doi.org/10.1126/science.aal4108

3. **van der Werf et al. (2006)** — Interannual variability and ENSO:
   van der Werf, G.R., et al. (2006). Interannual variability in global biomass burning emissions from 1997 to 2004. *Atmospheric Chemistry and Physics*, 6(11), 3423–3441.

---

## Data Citation
```
van der Werf, G.R., Randerson, J.T., Giglio, L., van Leeuwen, T.T., Chen, Y.,
Rogers, B.M., Mu, M., van Marle, M.J.E., Morton, D.C., Collatz, G.J., Yokelson, R.J.,
and Kasibhatla, P.S. (2017): Global fire emissions estimates during 1997–2016,
Earth Syst. Sci. Data, 9, 697-720, https://doi.org/10.5194/essd-9-697-2017
```
