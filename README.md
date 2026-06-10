# STELLAR CLASSIFICATION: SDSS SPECTRAL & PHOTOMETRIC ANALYSIS

## Overview

An end-to-end Machine Learning pipeline developed to classify astronomical observations into 
Galaxies, Stars, or Quasars (QSOs) using data from the Sloan Digital Sky Survey (SDSS). This
repository includes rigorous Exploratory Data Analysis (EDA), dimensionality reduction (PCA),
hyperparameter tuning via cross-validation, and multi-class model benchmarking.

## Performance Summary

The pipeline automatically benchmarks optimized models, selecting **XGBoost** as the top performer based on final metrics:
* **Accuracy:** **97.30%** (Surpasses target thresholds ≥ 93%)
* **Macro F1-Score:** **0.9686** (Surpasses target thresholds ≥ 0.92)
* **Macro ROC-AUC:** **0.9942**
* **Minimum Recall:** **0.9183** (All targeted classes cleanly exceed the ≥ 0.90 constraint)

## Repository Structure

```text
├── outputs/
│   ├── 01_class_distribution.png
│   ├── 02_feature_distributions.png
│   ├── 03_correlation_heatmap.png
│   ├── 04_redshift_boxplot.png
│   ├── 05_pca_2d.png
│   ├── 06_pca_scree.png
│   ├── 07_confusion_matrix.png
│   ├── 08_per_class_metrics.png
│   ├── 09_roc_curves.png
│   ├── 10_feature_importances.png
│   └── 11_model_comparison.png
├── Stellar_classification.ipynb   # Main Jupyter Notebook pipeline
└── requirements.txt               # Dependencies 
```
## Core Pipeline Features

### 1. Data Cleaning & Feature Selection

●Data Footprint: Processes a sampled space of 20,000 stellar objects to counter class imbalance challenges while preserving statistical characteristics.

●Irrelevant Feature Drops: Eliminates identification metadata and engineering artifacts (obj_ID, run_ID, rerun_ID, cam_col, field_ID, spec_obj_ID, plate, MJD, fiber_ID).

●Features Kept: * Spatial: alpha (Right Ascension), delta (Declination)

●Photometric (UGRIZ filters): u, g, r, i, z

●Spectroscopic: redshift

### 2. EDA
The system automatically populates the outputs/ folder with high-contrast, atmospheric visualizations mapped to specific analysis stages:

●01_class_distribution.png: Highlights the underlying data distribution across classes (Galaxy: 11,860, Star: 4,343, Quasar [QSO]: 3,797).

●02_feature_distributions.png: Overlays density distributions for each of the 8 features across classes.

●03_correlation_heatmap.png: Matrix tracking collinearity between the UGRIZ filters.

●04_redshift_boxplot.png: Isolates redshift as the single most critical discriminating metric separating systemic extragalactic features.

### 3.Dimensionality Reduction (PCA)

●Features are scaled dynamically using a standard scaler before applying projection techniques.

●05_pca_2d.png: Illustrates a 2-component spatial projection capturing 70.5% of the cumulative variance.

●06_pca_scree.png: Tracks the scree variance cliff and models the component inflection point required to crack the 95% threshold rule.

## Modeling & GridSearch Optimization

The system performs hyperparameter validation over 3 Cross-Validation folds (GridSearchCV) optimized natively for a macro F1 metric:

### XGBoost Configuration (Best Model)
●Tuning Space: n_estimators: [200, 300], max_depth: [6, 8], learning_rate: [0.1, 0.2], subsample: [0.8, 1.0]

●Best Parameters Obtained: {'learning_rate': 0.1, 'max_depth': 6, 'n_estimators': 300, 'subsample': 1.0}

●Score achieved: 97.30% Accuracy | 0.9686 Macro F1

### Random Forest Configuration
●Tuning Space: n_estimators: [200, 300], max_depth: [None, 20], min_samples_split: [2, 5], class_weight: ['balanced']

●Best Parameters Obtained: {'class_weight': 'balanced', 'max_depth': None, 'min_samples_split': 5, 'n_estimators': 200}

●Score achieved: 97.00% Accuracy | 0.9652 Macro F1

## Evaluation Matrix Charts
The pipeline saves deep validation artifacts for the championship-tier model:

●07_confusion_matrix.png : Displays raw counts alongside row-normalized precision matrices to identify class confusion vectors.

●08_per_class_metrics.png : Side-by-side performance bar graph breakdown highlighting precision, recall, and localized F1 steps.

●09_roc_curves.png : One-vs-Rest ROC curve chart demonstrating immaculate Area Under the Curve metrics (Star: 0.9996, Galaxy: 0.9930, QSO: 0.9899).

●10_feature_importances.png : Horizontal breakdown mapping variance dependence, confirming redshift as the dominant predictive signal.

●11_model_comparison.png : Macroscopic performance showdown comparing Accuracy, Macro F1, and Area Under the Curve between competing classifiers.


## Final Metrics Matrix (XGBoost)
   
GALAXY :  Precision -> 0.9721 ;  Recall -> 0.9831 ;   F1-Score -> 0.9776

QSO :   Precision -> 0.9561 ;  Recall -> 0.9183  ;   F1-Score -> 0.9368

STAR :   Precision -> 0.9897 ;  Recall -> 0.9931  ;   F1-Score -> 0.9914
