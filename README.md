# SCADA-Based Predictive Maintenance for Wind Turbines Using the CARE Benchmark

This repository contains the code used in my dissertation on **SCADA-based predictive maintenance for wind turbines** using the **CARE benchmark dataset**.

The project develops and evaluates a **two-stage machine learning pipeline** for wind turbine fault detection across multiple wind farms with different turbine configurations and sensor architectures.

## Project Overview

The main goal of this work is to investigate how a predictive maintenance pipeline can be designed and evaluated rigorously across heterogeneous wind farms, and how sensor architecture affects achievable fault detection performance.

The repository includes code for:

- **Stage 1 anomaly detection** using a denoising MLP autoencoder
- **Stage 2 fault classification** using XGBoost
- **Leave-One-Event-Out (LOEO)** validation
- **Cross-farm transfer experiments**
- **Subsystem-level feature engineering**
- **CARE benchmark metric evaluation**
- Supporting analysis and visualisation scripts

## Research Focus

This dissertation investigates:

- whether a two-stage SCADA-based fault detection pipeline can perform competitively on the CARE benchmark
- how subsystem-level sensor availability influences detectability
- whether subsystem-specific modelling improves anomaly detection
- whether cross-farm transfer can be improved using subsystem-level aggregate features and per-farm normalisation
- the operational significance of fault periods and service-mode downtime

## Dataset

This work uses the **CARE benchmark dataset** introduced by Gück et al. (2024), a public multi-farm SCADA dataset containing labelled fault events and a standardised evaluation framework.

> **Note:**  
> The CARE dataset is **not included** in this repository.  
> You must obtain access to the dataset separately and place it in the expected directory structure before running the scripts.
