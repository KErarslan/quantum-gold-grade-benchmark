# Benchmarking Quantum and Classical Methods for 3D Gold Grade Estimation

**Kaan Erarslan** — Department of Mining Engineering, Kütahya Dumlupınar University, Turkey
ORCID: [0000-0002-1875-4009](https://orcid.org/0000-0002-1875-4009)
Manuscript under review, *Mathematical Geosciences* (Springer)

## Overview

This repository contains the reproducible analysis pipeline for a systematic benchmark of
quantum machine learning (QML) methods — Variational Quantum Circuits (VQC), Quantum Neural
Networks (QNN), Quantum Kernel Ridge Regression (QKRR), and Quantum Support Vector Machines
(QSVM) — against classical geostatistical and machine learning methods (exponential Kriging,
Random Forest, MLP) for three-dimensional gold grade estimation.

The dataset is 20 RC drillholes (28 assay intervals) from the Kalgoorlie Gold Project
Northern Zone, Western Australia (Riversgold Limited, ASX:RGL), with coordinates modified
by isotropic scaling for confidentiality. Spatial autocorrelation structure was confirmed
preserved under this modification via a Mantel test (Spearman r = 0.34, p < 0.001).

## Key results

| Method | Parameters | LOOCV RMSE (g/t Au) | R² |
|---|---|---|---|
| Exponential Kriging | 3 | 0.261 | 0.609 |
| VQC (L=5) | 15 | 0.272 | 0.577 |
| QNN (L=8) | 24 | 0.291 | 0.515 |
| Random Forest | ~1,800 | 0.293 | 0.510 |
| MLP (64,32) | 2,369 | 0.347 | 0.310 |
| QKRR | 28 | 0.427 | −0.044 |

No statistically significant difference in LOOCV RMSE was detected between any pair of the
five comparable methods (10 pairwise Wilcoxon signed-rank tests, corrected for multiple
comparisons via Bonferroni and Benjamini–Hochberg FDR). VQC (L=5, 15 trainable parameters)
achieves performance statistically indistinguishable from exponential Kriging using
approximately 120-fold fewer parameters than Random Forest.

VQC circuits were additionally validated on three IBM Quantum hardware backends
(`ibm_marrakesh`, `ibm_kingston`, `ibm_fez`), with noise-induced RMSE degradation of
+6.7–34.7% depending on backend and calibration state.

## Repository contents

```
├── QuantumAndGeostatistics_reproducible_pipeline.ipynb   # Main analysis notebook
├── requirements.txt                                      # Python dependencies
├── figures01-10                                          # Output figures (Fig. 1-10)
└── README.md
```

## Running the notebook

The notebook is organised into sequential, labelled modules and runs end-to-end in
Google Colab or a local Jupyter environment with no live IBM Quantum access required —
hardware-validation results (Fig. 8, Fig. 9) are included as recorded values from the
original hardware experiments, with the corresponding IBM Quantum job IDs documented
in code comments for independent verification.

1. Open the notebook in Google Colab (or install dependencies locally via
   `pip install -r requirements.txt`).
2. Run all cells sequentially, top to bottom.
3. Output figures (`fig01_study_area.png` … `fig10_mantel_test.png`) are generated in the
   working directory at 600 DPI, sans-serif styling per journal requirements.

Approximate runtime: 30–60 minutes on standard Colab hardware, dominated by the leave-one-out
retraining of VQC and QNN circuits (Module 5).

### Optional: live IBM Quantum hardware re-validation

To independently re-run the hardware validation against IBM Quantum (not required to
reproduce the manuscript's figures or tables), supply your own IBM Quantum API token as a
Colab secret named `IBM_TOKEN` and restore the hardware-validation cells from the notebook
history; an active IBM Quantum account and CRN instance are required.

## Data availability

The modified dataset is available from the corresponding author upon reasonable request,
subject to a confidentiality agreement with Riversgold Limited (ASX:RGL). Original data
source: Riversgold Limited (2022) Quarterly Activities Report — Kalgoorlie Gold Project.

## Citation

If you use this code, please cite:

> Erarslan K. Benchmarking Quantum and Classical Methods for 3D Gold Grade Estimation:
> From Variational Circuits to Geostatistics with NISQ Hardware Validation.
> *Mathematical Geosciences* (under review).

## License

Code released under the MIT License (see `LICENSE`). Data usage is subject to the
confidentiality terms described above and is not covered by this license.

## Related work

- 3D Variogram Surface Kriging (3D-VSK): https://github.com/KErarslan/3D-VSK-kriging
- Distance-Encoded Quantum Kernels for Spatial Estimation (SQK): https://github.com/KErarslan/sqk-spatial-kernels
