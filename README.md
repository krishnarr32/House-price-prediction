# House Price Prediction: Ridge, Lasso, ElasticNet & XGBoost Regression

> **Python-based regression project** predicting house sale prices using a corrected preprocessing pipeline and comparing six models — Linear Regression, Ridge, Lasso, ElasticNet, Random Forest, and XGBoost — achieving **Test R² ≈ 0.93**.

---

## Business Problem

Accurate house price prediction is critical for real estate valuation, mortgage lending, and investment decisions. This project builds a robust regression pipeline on the Ames Housing dataset (2,908 records, 79 features) to predict `SalePrice` — addressing real-world data quality issues including missing values, skewed distributions, and multicollinearity.

---

## Dataset

| Property | Detail |
|---|---|
| Dataset | Ames Housing (house_prices.csv) |
| Rows | 2,908 |
| Predictor columns | 79 features |
| Target | `SalePrice` (USD) |
| Source | BUSINFO 716 course dataset / Kaggle |

**Key predictors:** `OverallQual`, `GrLivArea`, `TotalBsmtSF`, `GarageCars`, neighbourhood, basement quality, garage type

---

## Key Data Corrections

The most important preprocessing decisions in this project:

| Issue | Wrong Approach | Correct Approach Used |
|---|---|---|
| `PoolQC`, `FireplaceQu`, `GarageQual` NaN | Drop columns | Recode NaN → `'None'` (absence of feature, not missing data) |
| `LotFrontage` NaN (genuinely missing) | Drop or zero-fill | Neighbourhood median imputation |
| `MSSubClass` (20, 60, 120...) | Treat as numeric | Recast to string — building-type codes, not ordered numbers |
| `Utilities` (99.9% identical) | Keep | Drop — near-zero variance, adds noise not signal |
| `SalePrice` right-skewed (skew ≈ 1.57) | Model raw | Log-transform → skew ≈ 0.04 |

> Recoding absence-encoding NaNs as `'None'` preserved signal from 93–99% of rows that were previously being discarded.

---

## Pipeline

```
Raw Data (2,908 rows × 79 features)
        │
        ▼
Data Cleaning & EDA
├── Log-transform SalePrice (skew 1.57 → 0.04)
├── Recode absence NaNs → 'None' category
├── Neighbourhood median imputation for LotFrontage
├── Drop Utilities (near-zero variance)
└── Recast MSSubClass to string
        │
        ▼
Feature Engineering & Preprocessing Pipeline
├── Numeric: SimpleImputer → StandardScaler
└── Categorical: SimpleImputer → OneHotEncoder
        │
        ▼
Model Comparison (6 models)
├── Linear Regression (baseline)
├── Ridge (α = 50)
├── Lasso (α = 0.0005)
├── ElasticNet (α = 0.001, l1 = 0.3)
├── Random Forest
└── XGBoost
        │
        ▼
Evaluation: RMSE, MAE, R² on held-out test set
```

---

## Model Results (Test Set — Log Price Scale)

| Model | Test RMSE | Test MAE | Test R² |
|---|---|---|---|
| **Ridge (α=50)** | ✅ Best | ✅ Best | **≈ 0.93** |
| **Lasso (α=0.0005)** | ✅ Best | ✅ Best | **≈ 0.93** |
| **ElasticNet (α=0.001)** | ✅ Best | ✅ Best | **≈ 0.93** |
| XGBoost | Competitive | Competitive | ≈ 0.93 |
| Random Forest | Higher | Higher | Lower |
| Linear Regression | Baseline | Baseline | Baseline |

> Ridge, Lasso, and ElasticNet matched XGBoost at **R² ≈ 0.93** after correct preprocessing — showing that proper data cleaning matters more than model complexity.

---

## Key Findings

1. **Data cleaning > model complexity** — corrected preprocessing allowed regularised linear models to match XGBoost
2. **Lasso and ElasticNet** zeroed out low-information one-hot columns automatically via L1 penalty
3. **ElasticNet** handled correlated neighbourhood dummies most gracefully (L1 + L2 combined)
4. **Random Forest** underperformed due to untuned hyperparameters vs the tuned linear models
5. **Top predictors:** `OverallQual`, `GrLivArea`, `TotalBsmtSF`, `GarageCars` — quality and size dominate
6. **Residuals** approximately symmetric around zero — confirms log-transform was appropriate

---

## Tools & Libraries

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-FF6600?style=flat)

- **Language:** Python
- **Models:** `sklearn` (LinearRegression, Ridge, Lasso, ElasticNet, RandomForest), `xgboost`
- **Pipeline:** `sklearn.pipeline.Pipeline`, `ColumnTransformer`
- **Preprocessing:** `SimpleImputer`, `OneHotEncoder`, `StandardScaler`
- **Evaluation:** RMSE, MAE, R² (`sklearn.metrics`)
- **Visualisation:** `matplotlib`
- **Data:** `pandas`, `numpy`

---

## Repository Structure

```
.
├── BUSINFO_716_Q2_Final.ipynb              # Full regression pipeline notebook
├── BUSINFO716_HousePrices_Presentation.pptx # Presentation slides
├── BUSINFO716_Speaker_Script.docx           # Speaker notes
└── README.md
```

---

## How To Run

1. Open `BUSINFO_716_Q2_Final.ipynb` in **Jupyter Notebook** or **VS Code**
2. Place `house_prices.csv` in the same directory
3. Run all cells in order
4. All packages install automatically on first run

---

## Academic Context

- **Course:** BUSINFO 716 — Predictive Analytics, University of Auckland
- **Assignment:** Assignment 3 — Question 2
- **Type:** Individual project

---

*Project by Sai Krishna Sudarsan | Master of Business Analytics, University of Auckland*
