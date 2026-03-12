# Power Outage ML Forecasting — CSU-MLP Methodology

**Project:** Can Machine Learning Forecast Power Outage Factors?  
**Approach:** Reconstructed CSU-MLP Random Forest framework (Hill et al. 2023)  
**Pilot Domain:** FEMA Region 5 (IL, IN, MI, MN, OH, WI)

---

## Setup

```bash
# 1. Create virtual environment
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate

# 2. Open in VS Code
code .

# 3. Select the venv kernel in VS Code, then run:
#    notebooks/00_project_setup.ipynb  (installs all packages)
```

---

## Notebook Pipeline

| # | Notebook | Deadline | Status |
|---|---|---|---|
| 0 | `00_project_setup.ipynb` | — | Install deps + config |
| 1 | `01_data_ingestion.ipynb` | Mar 1 | Download GEFSv12 + EAGLE-I |
| 2 | `02_geospatial_integration.ipynb` | Apr 5 | Spatial mapping + feature engineering |
| 3 | `03_modeling.ipynb` | Apr 26 | RF training + validation |
| 4 | `04_interpretability.ipynb` | May 6 | Tree Interpreter + SHAP maps |

---

## Key Design Decisions

### Why not import the CSU-MLP source directly?
The operational CSU-MLP code is **proprietary** (confirmed on the CSU website).
The public-facing resources are:
- `eabarnes1010/ml_tutorial_csu` — a general Earth science ML tutorial (TF/Keras), not the RF model
- `csteele2/Wx4Colab CSU_MLP.ipynb` — a *viewer* that fetches/displays CSU operational forecast images

This project **reconstructs the methodology** from Hill et al. (2023) using scikit-learn's `RandomForestClassifier`, which is the same underlying algorithm.

### AWS Credentials for GEFSv12
GEFSv12 on AWS is a requester-pays bucket. You need:
```bash
pip install awscli
aws configure
# Enter your AWS Access Key ID and Secret Access Key
```

### Synthetic Data Mode
Until GEFS data is downloaded, Notebooks 3 & 4 run on **synthetic data** 
(generated in Notebook 2) so the full pipeline can be tested end-to-end.

---

## Data Sources

| Dataset | Access |
|---|---|
| GEFSv12 Reanalysis | `s3://noaa-gefs-retrospective` (requester-pays) |
| EAGLE-I Outages | https://doi.org/10.6084/m9.figshare.24237376 |
| US County Shapefile | https://www2.census.gov/geo/tiger/TIGER2022/COUNTY/ |
| NLCD Land Cover | https://www.mrlc.gov (manual download) |

---

## References

- Hill, Schumacher & Jirak (2023). *WAF* — GEFSv12 RF severe weather
- Alpay et al. (2020). *MDPI* — Outage prediction with HRRR
- Saki et al. (2025). *Nature Sci Rep* — Compound events & EAGLE-I
- Mazurek et al. (2025). *WAF* — Tree Interpreter explainability
- Tervo et al. (2021). *NHESS* — Object-based storm tracking
