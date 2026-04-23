# SCADA-Based Predictive Maintenance for Wind Turbines Using the CARE Benchmark

This repository contains the **Jupyter Notebook** on **SCADA-based predictive maintenance for wind turbines** using the **CARE benchmark dataset**.

The notebook includes the full workflow.

## Project Summary

This project investigates a **two-stage machine learning pipeline** for wind turbine predictive maintenance using SCADA data.

The work focuses on:

- detecting abnormal turbine behaviour from normal operating data
- classifying fault types from fault-associated patterns
- evaluating performance across multiple wind farms
- analysing the effect of heterogeneous sensor configurations on detection performance

## Notebook Contents

All scripts and experiments are contained in **one Jupyter Notebook**, which includes:

1. data loading and preprocessing  
2. sensor renaming and feature mapping  
3. anomaly detection modelling  
4. fault classification modelling  
5. cross-farm transfer validation  
6. metric calculation and visualisation  

## Dataset

This project uses the **CARE benchmark dataset**.

> **Note:**  
> The CARE dataset is **not included** in this repository.  
> You must obtain the dataset separately and place it in the required folder structure before running the notebook.

Base directory used in the notebook:

```python id="1smkgv"
D:\files\project\CARE_To_Compare
