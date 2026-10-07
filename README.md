# SCADA-Based Predictive Maintenance for Wind Turbines Using the CARE Benchmark

> **There is a version 2 of this project.** I rebuilt it from scratch with a stricter evaluation
> protocol, a tested Python package and uncertainty estimates:
> [care-wind-turbine-predictive-maintenance-v2](https://github.com/PeterImoniose/care-wind-turbine-predictive-maintenance-v2).
> This repository is kept as the work that was submitted and graded. The v2 README explains what
> changed and why its scores are lower.

This repository contains the Jupyter Notebook behind my MSc dissertation on SCADA-based
predictive maintenance for wind turbines, evaluated on the CARE benchmark dataset. It was
awarded 81% for the written dissertation and 90% for the practical engineering component, and
the underlying research has been submitted for peer review at the euspen Special Interest
Conference on Precision Engineering for Sustainability (SIC 2026).

## Project Summary

A machine learning pipeline for wind turbine predictive maintenance using 10-minute SCADA sensor
data from the three wind farms (A, B, C) of the CARE benchmark:

- **Stage 1 - Anomaly detection.** A per-farm Normal Behaviour Model: StandardScaler -> PCA
  (95% variance retained) -> a denoising autoencoder implemented in NumPy, trained on normal
  operating data. An alarm is raised when the reconstruction error stays above a per-farm
  threshold. On Farm B, additional subsystem-level models (for example transformer and gearbox
  bearing sensors) are fused with the global model. Evaluated with the benchmark's composite
  metric: **C**overage, **A**ccuracy, **R**eliability and **E**arliness, combined into one CARE
  score per farm. An Isolation Forest on the same features is included as a comparison.
- **Stage 2 - Fault classification.** An XGBoost classifier predicts the fault type, using
  sensor renaming and subsystem grouping, feature engineering, and oversampling of rare fault
  types. Validated with Leave-One-Event-Out (LOEO) cross-validation per farm, reporting
  detection rate, classification accuracy and warning horizon in days.
- **Cross-farm transfer experiment.** Tests whether a classifier trained on one farm's sensors
  still recognises faults on another farm, using subsystem-level aggregate features and per-farm
  normalisation, across five source-to-target directions.

## Results as reported in the dissertation

**Stage 1 - CARE score (denoising autoencoder)**

| Farm | CARE | Coverage | Accuracy | Reliability | Earliness | Faults detected | False alarms |
|---|---|---|---|---|---|---|---|
| A | 0.608 | 0.261 | 0.989 | 0.714 | 0.087 | 4 of 12 | 0 of 10 |
| B | 0.626 | 0.302 | 0.963 | 0.769 | 0.132 | 3 of 6 | 1 of 9 |
| C | 0.731 | 0.463 | 0.981 | 0.597 | 0.634 | 8 of 27 | 2 of 31 |

The Isolation Forest comparison scored 0.518, 0.611 and 0.733 on Farms A, B and C. The benchmark
authors report 0.66 for their autoencoder, pooled over all datasets.

**Stage 2 - Fault classification (LOEO)**

| Farm | Detection rate | Classification accuracy | Average warning horizon |
|---|---|---|---|
| A | 6 of 12 (50.0%) | 4 of 12 (33.3%) | 12.5 days |
| B | 5 of 6 (83.3%) | 2 of 6 (33.3%) | 41.1 days |
| C | 12 of 27 (44.4%) | 6 of 27 (22.2%) | 19.3 days |

**Cross-farm transfer**

| Direction | Comparable fault types detected | All fault types detected |
|---|---|---|
| Farm A to Farm C | 1 of 5 (20.0%) | 1 of 27 (3.7%) |
| Farm C to Farm A | 0 of 12 | 0 of 12 |
| Farm C to Farm B | 0 of 6 | 0 of 6 |
| Farm B to Farm A | 1 of 12 (8.3%) | 1 of 12 (8.3%) |
| Farm B to Farm C | 0 of 27 | 0 of 27 |

## Known limitations

When I rebuilt this project as v2 I found that the evaluation here was more generous than it
should have been. The numbers above are what the dissertation reported, and they should be read
with these points in mind:

- **Stage 1 training data was chosen using the event labels.** Each farm's model was trained on
  the datasets labelled "normal" and then scored on those same turbines alongside the fault
  datasets. The benchmark protocol is one model per dataset with the label unseen.
- **Thresholds, epochs and persistence windows were set per farm** on the same events that were
  then scored, with no held-out data.
- **The CARE score was my own implementation** and was not checked against the benchmark
  authors' code. It used a criticality threshold of 36, where the published metric uses 72.
- **The comparison with 0.66 is not like-for-like.** The dissertation compared it with the
  average of three farm scores; the published figure is pooled over all 95 datasets.
- **Stage 2 "detection" counts an event as detected even when the predicted fault type is
  wrong**, and eight fault types occur only once, so they can never be predicted under LOEO.
- **Minimum, maximum and standard deviation columns were used as inputs.** The dataset authors
  document these as unreliable, especially on Farm B.

Under a protocol without these issues, v2 scores the equivalent autoencoder at 0.578, 0.521 and
0.632 on Farms A, B and C.

## Notebook Contents

`wind turbine3.ipynb` holds the whole pipeline as a sequence of scripts, one per cell:

1. Stage 1 anomaly detection with the denoising autoencoder and CARE scoring.
2. The Isolation Forest version of Stage 1, used as the comparison.
3. Stage 2 fault classification with LOEO evaluation.
4. Cross-farm transfer validation.
5. An alternative cross-farm variant that was not used in the dissertation.
6. Operational analysis, summary tables and dissertation figures (remaining cells).

Each stage reads the outputs of the one before it, so the notebook has to be run top to bottom.
Figures are written to disk rather than shown inline. A full run took about 22 hours on a laptop
CPU.

## Dataset

This project uses the **CARE to Compare** benchmark dataset
([Zenodo](https://doi.org/10.5281/zenodo.14006163)): Gück, C., Roelofs, C. M. A. and
Faulstich, S. (2024), *Data*, 9(12), 138.

> **Note:**
> The dataset is **not included** in this repository. Download it separately and place it in the
> folder structure below before running the notebook.

The notebook expects the dataset at this hardcoded path, which you will need to change:

```
D:\files\project\CARE_To_Compare
```

with the three folders `Wind Farm A`, `Wind Farm B` and `Wind Farm C` inside it, plus a combined
event table `df_all__1_.csv` built from the three `event_info.csv` files.
