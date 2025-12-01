# TimeGAN_CEP_Household
# TimeGAN Experimental Study – Household Power Consumption (CEP Project)

This repository contains the implementation, experimental analysis, and results for the **BDA CEP Project (Part-2)**, extending the earlier survey paper on *Temporal Dynamics in Time Series Using GANs*.  
The goal is to train **TimeGAN** on real household electricity data and evaluate how well it captures temporal patterns.

---

## Project Overview

This project demonstrates:

- Preprocessing and windowing multivariate time series  
- Training TimeGAN on a small real-world dataset  
- Generating synthetic sequences  
- Visualizing and analyzing temporal dynamics  
- Computing quantitative metrics (DTW, MMD, Predictive Score)  
- Comparing real vs synthetic behavior  

All experiments were done in **Google Colab (CPU-only)** with strict runtime and memory limits.  
We therefore used a **small subset (5000 rows train, 1000 rows test)** of the dataset.

---

## Repository Structure

.
├── TimeGAN_Household_Main.ipynb # Main Colab notebook
├── raw_data/
│ ├── household_train_5000.csv
│ ├── household_test_1000.csv
│ ├── synthetic_household.npy
│
├── figures/
│ ├── overlay_global_active_power_seq0.png
│ ├── hist_global_active_power_real_vs_synth.png
│ ├── acf_global_active_power.png
│ ├── pacf_global_active_power.png
│ ├── pca_real_vs_synthetic.png
│ ├── tsne_real_vs_synthetic.png
│ ├── decomposition_real_global_active_power.png
│ ├── decomposition_synthetic_global_active_power.png
│ ├── changepoints_real_global_active_power.png
│ ├── changepoints_synthetic_global_active_power.png
│ ├── lagged_xcorr_global_active_power_voltage.png
│ └── ... (more analytic figures)
│
└── README.md

## Dataset

We use the **Household Electric Power Consumption** dataset  
(https://www.kaggle.com/datasets/uciml/electric-power-consumption-data-set)

Selected variables:
- Global active power  
- Global reactive power  
- Voltage  
- Global intensity  
- Sub-metering 1, 2, 3  

Preprocessing steps:
- Date+Time merged into `datetime`
- Missing values removed  
- Sorted chronologically  
- Normalized per feature  
- Windowed into **24-step sequences** for TimeGAN  

---

## TimeGAN Configuration

- Model: **GRU-based TimeGAN**
- Sequence length: **24**
- Hidden dimension: **16**
- Layers: **2**
- Batch size: **64**
- Learning rate: **0.001**
- Epochs: **5** (small due to hardware limits)
- Device: CPU (Google Colab)

Synthetic sequences were generated from the trained TimeGAN generator + recovery network.

---

## Visualizations & Temporal Diagnostics

The `figures/` folder contains all analysis plots:

- Real vs Synthetic overlay  
- Real vs Synthetic distribution (histogram/KDE)  
- ACF & PACF comparison  
- PCA projection of sequences  
- t-SNE embedding plots  
- Trend & Seasonality decomposition (real + synthetic)  
- Change-point detection (Ruptures)  
- Lagged cross-correlation  

These reveal that TimeGAN learned **short-term fluctuations**, but **did not learn long-term seasonality or multivariate structure** on this small dataset.

---

## Quantitative Metrics

**DTW (Dynamic Time Warping):**
Mean DTW = 46.02
Std = 15.55

mathematica
Copy code
→ Real and synthetic sequences differ in shape.

**MMD (Maximum Mean Discrepancy):**
MMD = 0.665

scss
Copy code
→ Moderate distribution gap between real and synthetic data.

**Predictive Score (Next-Step Prediction):**
Train Synthetic → Test Real: MSE = 3.74
Train Real → Test Synthetic: MSE = 4.26

yaml
Copy code
→ Synthetic data does not preserve predictive structure from the real dataset.

---

## Key Findings

- TimeGAN can generate sequences with correct dimensions and rough local variation.  
- It does **not** capture the strong daily seasonality in electricity consumption when trained on a small dataset with limited epochs.  
- Multivariate relationships (e.g., active power ↔ voltage) are not learned well.  
- PCA, t-SNE, ACF, PACF, decomposition, and change-point detection all show a clear gap between real and synthetic dynamics.

**Overall conclusion:**  
TimeGAN is sensitive to dataset size and training duration. Larger datasets and longer training would likely improve results.

---

## How to Reproduce

1. Open the notebook:  
   `TimeGAN_Household_Main.ipynb`  
2. Run all cells (CPU or GPU)  
3. Outputs (synthetic data + plots) will be saved in `figures/` and `raw_data/`

---

## License
MIT License. You may reuse this code with attribution.

---

## Acknowledgments

- Original TimeGAN paper: *Jinsung Yoon, Daniel Jarrett, Mihaela van der Schaar (NeurIPS 2019)*  
- Data from Kaggle
- The project was developed and successfully executed by:
- **Dua Eman**
- **Rabia Ejaz**
- **Anila Khan**
- Project completed as part of **BDA CEP Coursework (2025)**  
