# Transformer-Based Models for Predictive Maintenance  

This repository contains the work from my **Bachelor Thesis**, where I explored how **transformer-based models** can be applied to predictive maintenance and compared their performance against traditional machine learning approaches.  

---

## 🎯 Objective  
The primary goal was to investigate whether transformers — specifically **PatchTST** and **MOMENT** — can effectively predict machine failures in advance. Predictive maintenance is crucial because anticipating failures before they happen reduces unexpected downtime, saves costs, and improves overall machine reliability and safety.  

---

## 📊 Dataset  

- **Source**: Microsoft Azure Predictive Maintenance dataset  
- **Scope**: One year of hourly data for 100 industrial machines  
- **Size**: 876,100 records with only **761 failures** (≈0.001% failure rate → extreme class imbalance)  
- **Sensors**:  
  - Voltage  
  - Rotation  
  - Pressure  
  - Vibration  
- **Additional Data**: Machine errors, maintenance logs, machine specs (age, model, etc.)  

### Key Insights from Data Analysis  
- **Independence**: Sensors were almost uncorrelated → each provided unique information.  
- **Outliers**: Rotation had the most variability, vibration the most stable → kept all outliers as they likely reflect real machine behavior.  
- **Distributions**: Sensors followed near-normal distributions.  
- **Machine factors**: Older machines and certain models (model1, model2) had significantly more failures. Machine #99 alone failed 19 times in one year.  

---

## ⚙️ Methodology  

### Prediction Task  
Formulated as **binary classification**:  
- Will a machine fail within the next **24, 48, or 72 hours**?  

### Data Preparation  
- Merged telemetry with machine specs and error logs.  
- Encoded categorical variables (machine model, error types).  
- Labeled rows with failure windows (24h, 48h, 72h).  
- Time-based splitting: **70% train, 15% validation, 15% test** to avoid leakage.  
- Addressed class imbalance with **class-weighted training** (gave failures more weight).  

### Models  
- **Traditional**: XGBoost (with rolling window statistics: mean, std, min, max over 6/12/24h).  
- **Transformers**:  
  - PatchTST  
  - MOMENT  
- Transformers directly consumed raw sequences (no manual feature engineering).  

---

## 🚀 Results  

- **XGBoost** performed well on engineered features but struggled with longer horizons (72h).  
- **PatchTST & MOMENT** outperformed traditional models, especially on longer horizons, by capturing complex temporal dependencies.  
- **Class-weighted training** improved recall on rare failures, making models more sensitive to minority cases.  
- Misclassification analysis revealed **machine-specific risks** (e.g., older machines or certain models more prone to false negatives).  

