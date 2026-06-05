# Kaggle: Product Return Prediction

**Drexel STAT 640 — Predictive Analytics & Machine Learning**  
**Team:** The Fellowship of the Mean

---

## Competition

Binary classification task: predict whether an e-commerce transaction results in a product return (`returned = 1`).

- **Training data:** ~Jan–Sep 2025 transactions
- **Test data:** Sep–Dec 2025 transactions
- **Metric:** AUC-ROC

---

## Approach

```
Raw Data → Preprocessing → Feature Engineering → Model Selection → Ensemble
```

1. **Preprocessing** (`preprocessing.R`) — timestamp decomposition, payment method normalization, dummy encoding, train/test alignment
2. **Baseline models** (`model_choice.R`) — logistic regression, LASSO, random forest, XGBoost compared via cross-validated AUC
3. **Feature engineering v2** (`feature_engineering_v2.R`) — customer-level aggregates, return rate history, product category signals
4. **XGBoost tuning** (`xg_boost_fine.R`, `xg_boost_v2.R`) — eta grid search, early stopping, fold-based CV
5. **Feature engineering v3** (`feature_engineering_v3.R`) — additional behavioral features, interaction terms
6. **CatBoost baseline** (`cat_boost_base.R`) — independent CatBoost pipeline for diversity in ensemble
7. **Final ensemble** (`cat_xg_blend.R`) — weighted blend of XGBoost and CatBoost out-of-fold predictions

---

## Stack

| Tool | Purpose |
|------|---------|
| R / dplyr | Data manipulation and feature engineering |
| xgboost | Gradient boosting (primary model) |
| catboost | Gradient boosting (ensemble diversity) |
| glmnet | LASSO baseline |
| randomForest | Random forest baseline |
| caret | Cross-validation framework |
| pROC | AUC evaluation |

---

## File Map

| File | Stage |
|------|-------|
| `preprocessing.R` | Data cleaning and feature extraction |
| `model_choice.R` | Baseline model comparison |
| `model_fold.R` | Cross-validation fold strategy |
| `feature_engineering_v2.R` | Customer and product aggregates |
| `xg_boost_fine.R` | XGBoost hyperparameter tuning |
| `v2_verification.R` | Model verification and leakage checks |
| `feature_engineering_v3.R` | Behavioral features and interaction terms |
| `modeling_v3.R` | Full modeling pipeline v3 |
| `xg_boost_v2.R` | XGBoost refit on v3 features |
| `cat_boost_base.R` | CatBoost baseline pipeline |
| `cat_xg_blend.R` | XGBoost + CatBoost ensemble blend |
