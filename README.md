# GSVA-based Tumor Aggressiveness Prediction in Prostate Cancer (TCGA-PRAD)

**Master's Thesis | Bioinformatics and Biostatistics | UOC**  
**Author:** José Pablo Delgado Navarro  
**Year:** 2026

---

## Overview

This repository contains the full reproducible pipeline for the analysis presented in the TFM:  
*"Predicción de agresividad tumoral en cáncer de próstata mediante integración de datos de expresión génica y rutas biológicas: un enfoque basado en GSVA y modelos de machine learning"*

The pipeline integrates RNA-seq expression data from the TCGA-PRAD cohort with pathway activity scores computed via Gene Set Variation Analysis (GSVA) over the 50 MSigDB Hallmark gene sets, and builds multinomial predictive models for tumor aggressiveness (Gleason-based: Low / Intermediate / High).

---

## Repository Structure

```
.
├── 01_descarga_TCGA_PRAD.Rmd      # Data download and curation from GDC (TCGAbiolinks)
├── 01_descarga_TCGA_PRAD.html     # Rendered output with interactive results
├── 02_preprocesado.Rmd            # RNA-seq preprocessing: filtering, VST normalization, PCA, outlier detection
├── 02_preprocesado.html           # Rendered output
├── 03_gsva_TCGA_PRAD.Rmd         # GSVA pathway scoring + differential activity analysis (limma)
├── 03_gsva_TCGA_PRAD.html        # Rendered output
├── 04_modelado.Rmd               # Multinomial predictive modeling (Clinical, GSVA, Elastic Net, Random Forest)
├── 04_modelado.html              # Rendered output
├── 04B_sensibilidad_ganglionar.Rmd  # Sensitivity analysis: impact of lymph node variable
└── 04B_sensibilidad_ganglionar.html # Rendered output
```

---

## Pipeline Summary

| Script | Description |
|--------|-------------|
| `01` | Downloads TCGA-PRAD RNA-seq counts and clinical data from GDC. Defines the tumor aggressiveness endpoint (Gleason ≤6 / =7 / ≥8). |
| `02` | Filters low-expression genes, applies VST normalization, runs PCA for batch effect assessment, and removes Mahalanobis outliers. |
| `03` | Computes GSVA scores for 50 MSigDB Hallmarks. Runs limma differential activity analysis between High and Low aggressiveness groups. |
| `04` | Trains and evaluates four multinomial models via repeated stratified cross-validation (k=5, r=5). Includes interpretability analysis (Elastic Net coefficients, Random Forest variable importance, SHAP values). |
| `04B` | Sensitivity analysis comparing models with (n=398) and without (n=462) the lymph node status variable. |

---

## Key Results

- **Best model:** Integrated Elastic Net (clinical + GSVA) — AUC = 0.717 ± 0.038
- **Clinical model:** AUC = 0.702 ± 0.038
- **GSVA-only model:** AUC = 0.675 ± 0.055
- **Significantly differentially active pathways** (limma, FDR < 0.05): ESTROGEN_RESPONSE_EARLY, ESTROGEN_RESPONSE_LATE, BILE_ACID_METABOLISM (all reduced in high-aggressiveness tumors)
- **Top predictors:** T stage, E2F_TARGETS, G2M_CHECKPOINT

---

## Requirements

- R ≥ 4.3
- Key packages: `TCGAbiolinks`, `DESeq2`, `GSVA`, `limma`, `glmnet`, `randomForest`, `caret`, `pROC`, `SHAPforxgboost`

All package dependencies are loaded at the top of each `.Rmd` script. Rendering requires internet access for the data download step (`01`); subsequent scripts use locally saved `.rds` files.

---

## Data Availability

Raw RNA-seq data is publicly available via the [GDC Data Portal](https://portal.gdc.cancer.gov/) (TCGA-PRAD project). The processed intermediate files (`.rds`) are not included in this repository due to size constraints but can be reproduced by running the scripts in order.

---

## Citation

If you use this pipeline, please cite:

> Delgado Navarro, J.P. (2026). *Predicción de agresividad tumoral en cáncer de próstata mediante integración de datos de expresión génica y rutas biológicas*. TFM, Máster Universitario en Bioinformática y Bioestadística, UOC.
