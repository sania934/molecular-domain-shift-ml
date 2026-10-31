# Molecular Domain Shift in Machine Learning for Electronic Property Prediction

## Overview

This project investigates how a machine-learning model trained on one molecular dataset behaves on another. It uses Morgan fingerprints and a Random Forest to predict an electronic-gap property for two datasets: a 5,000-molecule subset of QM9 (small organic molecules) and 180 organic-photovoltaic (OPV) molecules from HOPV.

The repository is a learning project. The in-domain QM9 result is sound. The cross-domain result has a units problem, described under Known Issues, and should not be read as it was first written.

---

## Known Issues

**The cross-domain R², MAE and RMSE values are not meaningful as reported.** The two targets are in different units:

* the QM9 `gap` column is in **Hartree** (mean 0.252, standard deviation 0.048, which is 6.86 ± 1.30 eV), and
* the HOPV target (`prop_7`) is on an **eV-like scale**. The saved cross-domain metrics imply a mean of about 1.8 and a standard deviation of about 0.34.

In both directions the average prediction is therefore wrong by the difference between the two scales. For QM9 → HOPV, the squared mean error (MAE² = 2.47) accounts for about 96% of the mean squared error (2.58). For HOPV → QM9 it accounts for about 100% (2.688 of 2.690). The very negative R² values (−22.0 and −1178.0) mainly reflect this scale difference, not a lack of chemical transferability, and the original explanation in terms of scaffold mismatch is not supported by these numbers.

**What is still valid.** The Pearson correlation is independent of units. It is about 0.08 in both directions, which indicates very little linear relation between the predictions and the targets of the other domain. The in-domain QM9 result is also unaffected.

**Status.** The experiment was not repeated with a common unit. To do so, convert the QM9 target to eV in notebook `03` (`y_qm9 = qm9["gap"] * 27.211386`), re-run notebooks `03` to `06`, and replace the cross-domain table below.

---

## Datasets

### QM9 subset (included)

`qm9_5k.csv` contains 5,000 molecules sampled from QM9 (the molecule identifiers range from `gdb_24` to `gdb_133869`, so the sample is not simply the first 5,000). The random seed used for the sampling was not recorded, so the file itself defines the subset. Properties used: `gap` (in Hartree). The file also contains the other QM9 properties (`homo`, `lumo`, `mu`, and others).

### HOPV subset (not included)

The HOPV file, `hopv_clean_target_prop7.csv`, is not distributed in this repository. It contains 180 molecules and the columns `SMILES`, `prop_5`, `prop_6` and `prop_7`, where `prop_7 = prop_6 − prop_5` (calculated in notebook `02`).

* **Source:** `[add the HOPV source and citation]`
* **Meaning of the properties:** `[add what prop_5 and prop_6 are, and their units]`
* **Selection:** `[add how the 180 molecules were selected or cleaned]`

To reproduce the results, build this file with these columns and place it next to the notebooks.

---

## Workflow

| Notebook | Purpose |
| --- | --- |
| `01_data_loading_and_exploration.ipynb` | Load both datasets, check shapes and missing values, plot target distributions |
| `02_preprocessing_and_feature_check.ipynb` | Calculate the target columns (`gap`, `prop_7`) and keep the SMILES and target columns. These results are not used by later notebooks, which reload the CSV files |
| `03_fingerprint_genertion.ipynb` | Generate 1024-bit Morgan fingerprints (radius 2) with RDKit and save `X_*.npy` and `y_*.npy` |
| `04_baseline_models.ipynb` | Train and evaluate a Random Forest on QM9 (80/20 split, `random_state=42`) |
| `05_cross_domain_exp.ipynb` | Train on one dataset and test on the other, in both directions |
| `06_visualization and analysis.ipynb` | PCA plot of the QM9 and HOPV fingerprints |

Only a Random Forest (200 trees) was used. The Random Forest in notebook `04` has no fixed seed, so repeated runs give slightly different numbers. In notebook `05` it uses `random_state=42`.

---

## Results

### QM9 baseline (in-domain)

| Metric | Value |
| --- | ---: |
| Train R² | 0.976 |
| Test R² | 0.839 |
| Test MAE | 0.0139 Hartree (about 0.38 eV) |

Saved in `rf_metrics.csv`.

### Cross-domain (as originally computed, in mixed units)

| Experiment | R² | MAE | RMSE | Pearson r |
| --- | ---: | ---: | ---: | ---: |
| QM9 → HOPV | −22.05 | 1.571 | 1.606 | 0.083 |
| HOPV → QM9 | −1177.99 | 1.639 | 1.640 | 0.078 |

Saved in `cross_domain_results.csv`. Only the Pearson column is valid (see Known Issues). The HOPV → QM9 model was trained on only 180 molecules.

---

## Limitations

* The cross-domain experiment mixes Hartree and eV targets (see Known Issues).
* Only Morgan fingerprints and a Random Forest were tested.
* One random split, one seed, and no uncertainty estimates.
* The HOPV set is small (180 molecules), and the QM9 subset was sampled without a recorded seed.
* The reasons for poor transfer (target offset, chemical differences, dataset size) were not separated.

---

## How to Reproduce

1. **Environment.** The notebooks were created in Google Colab (Python 3.12, NumPy 2.0.2). The other package versions were not recorded. Install the packages with `pip install -r requirements.txt`.
2. **Data.** `qm9_5k.csv` is included. Place the HOPV file as described above.
3. **Run** the notebooks in order, `01` to `06`, from this folder. They create the `.npy` and `.pkl` files locally, which are not tracked by git.
4. **Expected output.** Notebook `04` should give a test R² close to 0.84 and a test MAE close to 0.014 Hartree. Notebook `05` should reproduce the table above, unless the units are first made consistent.

---

## Author

Sania Ismaeel
