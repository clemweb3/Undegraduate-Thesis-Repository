# Regional Financial Vulnerability in the Philippines
**Labor Force Survey–Based Preprocessing, Factor Analysis, and Index Construction**

> **Associated Publication:** *Towards the Determination of Regional Financial Vulnerability in the Philippines Using Labor Force Indicators* — submitted to IBDAP 2026.

---

## Overview

This repository contains the complete analytical pipeline for constructing the **Regional Financial Vulnerability Index (RFVI)** from Philippine Statistics Authority (PSA) Labor Force Survey (LFS) microdata spanning 2018–2024 (40 survey months, ~6 million household-level observations). The pipeline operationalizes the theoretical S-R-E framework of Voith and Mauser (2024) through Factor Analysis of Mixed Data (FAMD), composite index construction, and K-means clustering, all delivered via an interactive policy dashboard.

The repository is designed for **full reproducibility**: every preprocessing decision, analytical choice, and intermediate result is versioned through Git commits and documented in sequentially numbered Jupyter notebooks.

---

## Key Findings

- **Three latent vulnerability dimensions** were extracted — Sensitivity (eigenvalue 14.32), Resilience (5.57), and Exposure (2.80) — explaining **32.86% of total variance** across 16 harmonized mixed-type variables.
- **NCR ranks as most financially vulnerable** among all 17 regions, driven by labor market *sensitivity* (high job-seeking and irregular employment), not geographic exposure — a distinction invisible to single-indicator measures.
- The **narrow inter-regional RFVI range** confirms financial vulnerability is a nationwide structural condition, not a geographically concentrated one.
- **K-means clustering (K=3)** identifies three structural typologies: Balanced S-R/Exposure-Contained, Exposure-Weighted/Resilience-Favored, and Sensitivity-Weighted/Resilience-Moderated.

---

## Repository Structure
.
├── 00_Settings.ipynb               # Centralized configuration (data paths, parameters)
├── 01_Metadata_Harmonization.ipynb
├── 02_Cross_Wave_Harmonization.ipynb
├── 03_Geographic_Reconstruction.ipynb
├── 04_Missingness_Diagnostics.ipynb
├── 05_Imputation_RF_MICE.ipynb
├── 06_FAMD_Pipeline.ipynb
├── 07_Dimension_Validation.ipynb
├── 08_RFVI_Construction.ipynb
├── 09_Clustering.ipynb
├── 10_Regional_Aggregation.ipynb
├── 11_Provincial_City_Aggregation.ipynb
├── 12_Visualization.ipynb
├── 13_Dashboard.ipynb
├── 14_Export_and_Outputs.ipynb
├── data/
│   ├── raw/                        # PSA LFS microdata (not included — see Data Access below)
│   ├── interim/                    # Intermediate outputs per pipeline stage
│   └── processed/                  # Final RFVI outputs by geographic level
├── outputs/
│   ├── figures/                    # Scree plots, RFVI rankings, cluster distributions
│   └── tables/                     # Cos² tables, cluster profiles, regional RFVI
└── dashboard/                      # Interactive app assets
---

## Quickstart: Reproducing the Analysis

### 1. Prerequisites

​```bash
pip install -r requirements.txt

# Core dependencies:
# pandas, numpy, scikit-learn, prince (FAMD), miceforest, matplotlib, seaborn, plotly
​```

### 2. Data Access

Raw LFS microdata is publicly available from the **Philippine Statistics Authority (PSA)**:

1. Visit: https://psada.psa.gov.ph/
2. Navigate to **Labor Force Survey** → select survey rounds (2018, 2019, 2022, 2023, 2024)
3. Download individual- and household-level microdata in CSV or Stata format
4. Place downloaded files under `data/raw/` following the folder naming convention in `00_Settings.ipynb`

> **Note:** Years 2020–2021 are excluded due to structural inconsistencies in survey implementation during the COVID-19 pandemic.

### 3. Configure Paths

Open `00_Settings.ipynb` and set your local data paths:

​```python
RAW_DATA_DIR  = "data/raw/"        # Path to downloaded LFS microdata
INTERIM_DIR   = "data/interim/"    # Intermediate outputs
PROCESSED_DIR = "data/processed/"  # Final RFVI outputs
​```

All downstream notebooks (`01_` through `14_`) import from this file. **No other notebook requires manual path configuration.**

### 4. Run the Pipeline

Execute notebooks sequentially:

​```bash
jupyter nbconvert --to notebook --execute 01_Metadata_Harmonization.ipynb
jupyter nbconvert --to notebook --execute 02_Cross_Wave_Harmonization.ipynb
# ... continue through 14_Export_and_Outputs.ipynb
​```

Or run interactively in JupyterLab in order from `01` to `14`.

---

## Methodology Summary

### Data Scope
- **Source:** PSA Labor Force Survey, 40 survey months across 2018, 2019, 2022, 2023, 2024
- **Scale:** ~6 million household-level observations
- **Variables retained:** 16 harmonized variables — 13 categorical, 3 numeric

### Preprocessing Pipeline
1. **Metadata harmonization** — codebooks reshaped to long-format lookup tables; age-variable inconsistencies resolved via two-pass decoding
2. **Cross-wave harmonization** — 16 variables passed consistency checks across all 40 survey months
3. **Geographic reconstruction** — province/city identifiers recovered via forward/backward fill within PSU-household groups; city-level data available from 2022 onward
4. **Missingness diagnostics** — FMI computed per variable per survey month; logistic regression used to assess MAR assumptions; work-related missingness recoded via logic rules
5. **Statistical imputation** — RF-MICE across 5 imputed datasets

### FAMD
- IncrementalPCA for computational scalability
- Eigenvalues, Cos² values, and dimension scores averaged across 5 imputations
- Scree plot elbow criterion used for retention (3 dimensions retained)
- Cos² threshold: 1/16 = 0.0625

### RFVI Construction
RFVI = (Sensitivity + (1 − Resilience) + Exposure) / 3
- Factor scores normalized via Min-Max scaling to [0, 1]
- Resilience inverted to reflect its protective role
- Equal weighting following Hasler et al. (2022)
- Aggregated at national, regional, provincial, and city levels per survey month

### Clustering
- K-means on standardized S-R-E scores at city-month level
- K=3 selected via WCSS elbow + Silhouette coefficient

---

## Dimension–Variable Map

| Variable | D1 Sensitivity | D2 Resilience | D3 Exposure |
|---|---|---|---|
| Employment status | **0.922** | 0.031 | 0.010 |
| Work indicator | **0.832** | 0.029 | 0.012 |
| Age | **0.807** | 0.028 | 0.012 |
| Job-seeking indicators | **0.798** | 0.002 | 0.000 |
| Sex | **0.760** | 0.011 | 0.006 |
| Relationship to HH head | **0.508** | **0.436** | 0.028 |
| Household size | **0.508** | **0.436** | 0.028 |
| Total hours worked | **0.486** | **0.415** | 0.004 |
| Marital status | **0.466** | **0.391** | 0.000 |
| Other job indicator | 0.021 | 0.025 | **0.357** |
| Region | 0.011 | 0.011 | **0.227** |
| Previous job / want more hours | 0.041 | 0.035 | **0.183** |

*Bold = above-average contribution (Cos² > 0.0625). Values averaged across 5 imputed datasets.*

---

## Cluster Profiles

| Cluster | Label | Sensitivity | Resilience | Exposure |
|---|---|---|---|---|
| 0 | Balanced S-R / Exposure-Contained | 0.255 | 0.336 | **0.206** (lowest) |
| 1 | Exposure-Weighted / Resilience-Favored | **0.249** (lowest) | **0.413** (highest) | **0.299** (highest) |
| 2 | Sensitivity-Weighted / Resilience-Moderated | **0.462** (highest) | **0.249** (lowest) | 0.233 |

---

## Interactive Dashboard

An interactive dashboard exposing S, R, and E as independently navigable layers alongside the composite RFVI is available at the repository link above. Supports filtering by region, province, city, and survey month — designed for policy targeting aligned with **SDG 8** and **SDG 10**.

---

## Limitations and Future Work

- Equal weighting is a current constraint; FIES integration may support empirical differential weighting
- City-level analysis limited to 2022 onward due to PSU rotation across master sample frames
- Restricting to working-age population (15+) would sharpen the Sensitivity dimension
- Longitudinal panel data would enable individual household trajectory analysis

---
---

## Ethical Considerations

All data are publicly available, anonymized LFS datasets from the PSA. No personally identifiable information is accessed or disclosed. This study is conducted strictly for academic and policy-oriented purposes.

---

## License

MIT License. See `LICENSE` for full terms.
