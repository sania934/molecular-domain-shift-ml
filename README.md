# Molecular Domain Shift in Machine Learning for Electronic Property Prediction

## Overview

This project investigates how machine learning models trained on molecular datasets generalize across different chemical domains. The study focuses on electronic property prediction using QM9 and HOPV datasets.

The project evaluates the effectiveness of Morgan fingerprint representations and analyzes performance degradation under cross-domain transfer learning scenarios.

---

## Objectives

- Build ML models for molecular property prediction
- Generate molecular fingerprints using RDKit
- Evaluate cross-domain generalization
- Analyze domain shift between QM9 and HOPV
- Compare baseline ML models

---

## Datasets

### QM9
- Small organic molecules (5000 molecules)
- Properties: HOMO, LUMO, band gap

### HOPV
- Organic photovoltaic molecules (180 molecules)
- More complex conjugated systems
- Properties: HOMO, LUMO, band gap
---

## Models Used
- Linear Regression
- Random Forest

---

## Workflow

1. Data loading and exploration  
2. Preprocessing and feature engineering  
3. Molecular fingerprint generation  
4. Baseline model training  
5. Cross-domain experiments  
6. Visualization and analysis  

---

## Files

- `01_data_loading_and_exploration.ipynb`
- `02_preprocessing_and_feature_generation.ipynb`
- `03_fingerprint_generation.ipynb`
- `04_baseline_models.ipynb`
- `05_cross_domain_exp.ipynb`
- `06_visualization_and_analysis.ipynb`

---

## Requirements

Install dependencies using:

```bash
pip install -r requirements.txt
```

---

## Key Features

- Morgan fingerprints (1024-bit)
- Cross-domain evaluation
- PCA visualization

## Results

### QM9 Baseline Performance

The Random Forest model trained on Morgan fingerprints achieved:

- Train R²: 0.976
- Test R²: 0.840
- MAE: 0.0139

These results indicate that fingerprint-based representations can effectively capture molecular patterns within the QM9 chemical domain.

### Cross-Domain Analysis

Cross-domain experiments were performed to evaluate how well models trained on one chemical space generalize to another.

Experiments included:
- QM9 → HOPV
- HOPV → QM9

The study investigates domain shift effects and scaffold mismatch between small organic molecules and OPV-related molecular systems.

## Cross-Domain Findings

Although the model achieved strong in-domain performance on QM9, cross-domain transfer from QM9 to HOPV resulted in highly negative R² values.

This suggests that Morgan fingerprint representations trained on small-molecule QM9 chemistry fail to generalize to OPV-related molecular systems due to substantial scaffold and domain mismatch.

These findings highlight the limitations of fixed fingerprint representations for transferability across chemically distinct datasets.
---

## Author

Sania Ismaeel