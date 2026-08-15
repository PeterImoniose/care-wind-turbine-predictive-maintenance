# SCADA-Based Predictive Maintenance for Wind Turbines Using the CARE Benchmark

This repository contains the Jupyter Notebook behind my MSc dissertation on SCADA-based
predictive maintenance for wind turbines, evaluated on the CARE benchmark dataset. It was
awarded 80% for the written dissertation and 90% for the practical engineering component, and
the underlying research has been submitted for peer review at the euspen Special Interest
Conference on Precision Engineering for Sustainability (SIC 2026).

## Project Summary

A three-stage machine learning pipeline for wind turbine predictive maintenance using SCADA
sensor data across multiple wind farms (Farm A, B, C in the CARE benchmark):

- **Stage 1 - Anomaly detection.** A per-farm Normal Behaviour Model (NBM) built from
  StandardScaler -> PCA (95% variance retained) -> Isolation Forest, trained only on normal
  operating data. Global (whole-turbine) and subsystem-level models (sensors grouped by
  subsystem - e.g. gearbox, transformer - via a sensor registry and grouping rules) are fused,
  with adaptive per-farm alarm thresholds. Evaluated using the CARE benchmark's own composite
  metric: **C**overage, **A**ccuracy, **R**eliability, and **E**arliness, combined into a single
  CARE score per farm.
- **Stage 2 - Fault classification.** An XGBoost classifier identifies *which* fault type an
  already-flagged anomaly is, using sensor renaming/grouping, feature engineering, and class
  oversampling for rare fault types. Validated with Leave-One-Event-Out (LOEO) cross-validation
  per farm, reporting detection rate (both a 5-consecutive-point rule and a last-10%-of-window
  rule), classification accuracy, and mean early-warning horizon in days.
- **Stage 3 - Cross-farm transfer validation.** Tests whether a model trained on one wind
  farm's sensor configuration still detects faults on a different farm's turbines, reporting
  detection rate and average warning horizon per transfer direction - directly addressing how
  heterogeneous sensor configurations across farms affect detection performance.

## Notebook Contents

The notebook contains the full pipeline: data loading and preprocessing, sensor renaming and
subsystem grouping, the Stage 1 anomaly/NBM models and CARE scoring, the Stage 2 fault
classifier and LOEO evaluation, Stage 3 cross-farm transfer validation, and the plots and
summary tables generated from all three stages (an earlier numpy-implemented autoencoder
version of the Stage 1 model is also retained in the notebook as part of that development
process).

Because each stage reads the outputs of the one before it (Stage 2 depends on Stage 1's
results, Stage 3 depends on both), the notebook needs to be run top to bottom - the specific
CARE scores, detection rates, and horizons are computed at run time and written out to CSV/JSON
report files rather than hardcoded anywhere, so they aren't reproduced here.

## Dataset

This project uses the **CARE benchmark dataset**.

> **Note:**
> The CARE dataset is **not included** in this repository. You must obtain the dataset
> separately and place it in the required folder structure before running the notebook.

Base directory used in the notebook:

```
D:\files\project\CARE_To_Compare
```

