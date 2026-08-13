# 🏥 Hospital Inpatient Cost Prediction

**Predicting hospital inpatient charges with machine learning** — helping healthcare providers forecast costs, plan resources, and support billing/insurance decisions before or during a patient's stay.

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?logo=pandas&logoColor=white)
![Scikit--learn](https://img.shields.io/badge/Scikit--learn-Baselines-F7931E?logo=scikitlearn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-Final%20Model-83a603?logo=xgboost&logoColor=white)
![LightGBM](https://img.shields.io/badge/LightGBM-Benchmarked-9ACD32)
![CatBoost](https://img.shields.io/badge/CatBoost-Benchmarked-FFCC00)
![TensorFlow](https://img.shields.io/badge/TensorFlow-ANN-FF6F00?logo=tensorflow&logoColor=white)
![SHAP](https://img.shields.io/badge/SHAP-Explainability-8A2BE2)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

> ⭐ **8 regression algorithms benchmarked end-to-end** — Linear, Ridge, Lasso, Decision Tree, Random Forest, XGBoost, LightGBM, CatBoost — plus a tuned TensorFlow ANN. **XGBoost was selected as the final model** after tuning, but every model above was trained, evaluated, and compared on the same pipeline.

---

## 📖 Table of Contents

- [Problem](#-problem)
- [Business Value](#-business-value)
- [Dataset](#-dataset)
- [Project Workflow](#-project-workflow)
- [Models Benchmarked](#-models-benchmarked)
- [Results](#-results)
- [Why XGBoost Won](#-why-xgboost-won)
- [Explainability](#-explainability-shap)
- [Key Insights](#-key-insights)
- [Repo Structure](#-repo-structure)
- [How to Run](#-how-to-run)
- [Tech Stack](#-tech-stack)
- [Future Work](#-future-work)

---

## 📌 Problem

Hospitals sit on huge volumes of admission, diagnosis, and treatment data — but struggle to translate it into accurate cost forecasts. This project builds a regression pipeline that predicts **Total Charges** for an inpatient using demographic, clinical, and admission features, and surfaces the key cost drivers behind those predictions.

## 💡 Business Value

| Outcome | Impact |
|---|---|
| 📊 Financial planning | More accurate hospital budgeting and forecasting |
| 💰 Pre-treatment estimates | Estimate costs before/during a patient's stay |
| 🧾 Insurance & billing | Support faster, data-backed claim processing |
| 🚨 Risk flagging | Identify high-cost patient groups early |
| 🏗️ Resource allocation | Data-driven staffing and capacity planning |

## 🗂 Dataset

| Attribute | Value |
|---|---|
| Dataset | [SPARCS Hospital Inpatient Discharges](https://health.data.ny.gov/) |
| Source | New York State Department of Health |
| Rows | 32,033 |
| Columns | 34 |
| Target | `Total Charges` |
| Problem type | Regression |

## 🔧 Project Workflow

This notebook follows a 23-phase, end-to-end pipeline:

```
1. Business Problem              → 9.  Data Understanding        → 17. Feature Importance
2. Project Objective             → 10. Data Cleaning             → 18. SHAP Analysis
3. Business Value                → 11. EDA                       → 19. ANN Modeling
4. ML Problem Statement          → 12. Feature Engineering       → 20. Final Model Comparison
5. Dataset Overview              → 13. Data Preprocessing        → 21. Business Insights
6. Project Workflow              → 14. Model Benchmarking (x8)   → 22. Conclusion
7. Importing Libraries           → 15. XGBoost Hyperparameter     → 23. Future Work
8. Load Dataset                      Tuning (RandomizedSearchCV)
                                  → 16. Model Diagnostics
```

| Phase | What was done |
|---|---|
| Data Cleaning | Removed high-missing columns, imputed categoricals, parsed currency fields |
| EDA | Analyzed cost drivers across age, admission type, severity, and payer |
| Feature Engineering | Ordinal encoding (severity/risk), binary encoding, high-cardinality grouping |
| Modeling | 8 algorithms benchmarked on an identical train/test split and preprocessing pipeline |
| Tuning | `RandomizedSearchCV` hyperparameter search on XGBoost |
| Diagnostics | Residual analysis, error distribution across cost bands |
| Explainability | Feature importance + SHAP analysis on the final tuned model |

## 🤖 Models Benchmarked

All 8 models below were trained and evaluated on the **same** features, split, and metrics — nothing was cherry-picked going in:

| # | Model | Family |
|---|---|---|
| 1 | Linear Regression | Linear |
| 2 | Ridge Regression | Linear (L2 regularized) |
| 3 | Lasso Regression | Linear (L1 regularized) |
| 4 | Decision Tree | Tree-based |
| 5 | Random Forest | Ensemble (bagging) |
| 6 | **XGBoost** ⭐ | Ensemble (boosting) — *final model* |
| 7 | LightGBM | Ensemble (boosting) |
| 8 | CatBoost | Ensemble (boosting) |
| + | TensorFlow ANN | Deep learning |

## 📊 Results

| Model | Test MAE | Test RMSE | Test R² |
|---|---:|---:|---:|
| Linear Regression | 6,789.23 | 11,965.11 | 0.6796 |
| Ridge Regression | 6,726.49 | 11,856.44 | 0.6725 |
| Lasso Regression | 6,726.62 | 11,857.06 | 0.6724 |
| Decision Tree | 6,063.91 | 12,969.71 | 0.6081 |
| Random Forest | 5,166.24 | 11,129.92 | 0.7114 |
| **XGBoost (tuned)** ⭐ | **4,840.42** | **10,160.79** | **0.7594** |
| LightGBM | 4,884.99 | 10,347.54 | 0.7505 |
| CatBoost | 5,096.57 | 10,372.12 | 0.7493 |
| ANN (TensorFlow) | 5,348.15 | 10,599.60 | 0.7382 |

<!-- Add exported chart images here, e.g.: -->
<!-- ![Model Comparison](images/model_comparison.png) -->
<!-- ![SHAP Summary](images/shap_summary.png) -->

## 🏆 Why XGBoost Won

Out of all 8 traditional models **and** a tuned neural network, XGBoost had the lowest error and highest R² after hyperparameter tuning with `RandomizedSearchCV`. It edged out the next-best boosting models (LightGBM, CatBoost) and clearly outperformed the linear baselines and single decision tree — which is why it was promoted to the final, deployed model. The badge at the top reflects that final choice; the table above reflects the full comparison.

## 🔍 Explainability (SHAP)

The final XGBoost model was interpreted using SHAP values and native feature importance to understand *why* it predicts what it predicts, not just *what*.

## 📈 Key Insights

- **Length of Stay** is the single strongest predictor of charges — longer stays drive higher costs.
- **Medical vs. Surgical classification** matters a lot: surgical cases push predictions up, medical cases down.
- **Severity of illness and risk of mortality** meaningfully shift cost predictions.
- The model struggles most with extreme high-cost outlier cases — residual spread increases at the top end.

## 🗂️ Repo Structure

```
hospital-inpatient-cost-prediction/
│
├── data/                         # Dataset & documentation
│   ├── README.md
│   └── Hospital_Inpatient_Discharges_(SPARCS_De-Identified)__2009_20260723.csv
│
├── images/                       # Project visualizations
│
├── hospital_inpatient_cost_prediction.ipynb
│                                  # Complete ML workflow
│
├── requirements.txt              # Python dependencies
├── LICENSE                       # MIT License
└── README.md                     # Project documentation
```

## ▶️ How to Run

```bash
git clone https://github.com/<your-username>/hospital-inpatient-cost-prediction.git
cd hospital-inpatient-cost-prediction
pip install -r requirements.txt
jupyter notebook notebooks/hospital-inpatient-cost-prediction-analytics.ipynb
```

## 🛠️ Tech Stack

`Python` · `Pandas` · `NumPy` · `Scikit-learn` · `XGBoost` · `LightGBM` · `CatBoost` · `TensorFlow/Keras` · `SHAP` · `Matplotlib` · `Seaborn`

## 🚀 Future Work

- Deploy the final model as an API for real-time cost prediction
- Validate against newer/external hospital datasets for generalizability
- Further feature engineering to reduce error on high-cost outliers

---

⭐ If you found this useful, consider starring the repo!
