# Kaggle: Product Return Prediction
**Drexel STAT 640 · The Fellowship of the Mean**

Binary classification challenge: predict whether an e-commerce transaction results in a product return. Hosted as a private Kaggle competition for Drexel's LeBow School of Business.

---

## Competition

| | |
|---|---|
| **Course** | STAT 640 — Predictive Analytics & Machine Learning |
| **School** | LeBow College of Business, Drexel University |
| **Instructor** | Oliver Schaer |
| **Team** | The Fellowship of the Mean |
| **Task** | Binary classification — `returned`: 0 or 1 |
| **Metric** | AUC-ROC |
| **Public result** | Not stated; score and leaderboard claims require the private submission history |
| **Deadline** | March 20, 2026 |

## My Contribution

Built and iterated the repository's modeling pipeline across preprocessing, cross-validation, leakage checks, feature engineering, XGBoost, CatBoost, and the final ensemble submission.

This was a team competition. The repository's dated commit history records my implementation work; the private submission history is the required source for any future score or leaderboard claim.

---

## Problem

An e-commerce retailer is losing net revenue to high product return rates and rising reverse logistics costs. The competition required:

1. A **binary classifier** predicting whether a purchased item will be returned
2. Identification of the **key drivers** behind returns
3. **Operational recommendations** to reduce return rates, grounded in model findings

**Training data:** Jan–Sep 2025 transactions  
**Test data:** Sep–Dec 2025 transactions  
**Features include:** transaction timestamps, geographic codes, product category/subcategory, pricing and discount depth, customer loyalty tier, account age, checkout method, promo codes, marketing channel, review scores, payment method

---

## Progression

### Mar 9 — Preprocessing pipeline
- Standardized dirty payment method values (`"credit card - Visa"` → `"Visa"`, etc.)
- Decomposed timestamps into hour of day, day of week, weekend flag, and time bucket (night / morning / afternoon / evening)
- Dummy-encoded categoricals; aligned train/test column sets
- Output: `train_clean.csv` / `test_clean.csv`

### Mar 10 — Baseline model comparison
- Ran four baselines via 5-fold cross-validated AUC: logistic regression, LASSO, random forest, XGBoost
- XGBoost led the pack; logistic/LASSO useful as sanity checks
- Established submission pipeline to Kaggle

### Mar 11 — Feature engineering v2 + XGBoost tuning
- Added customer-level aggregates: historical return rate, purchase frequency
- Built consistent fold strategy for reproducible evaluation across model iterations
- Tuned XGBoost via eta / max_depth / subsample grid search with early stopping

### Mar 15 — Model verification
- Audited v2 features for data leakage and target contamination
- Confirmed holdout AUC was stable across fold splits

### Mar 16 — Feature engineering v3 + refit
- Added behavioral interaction features (discount × loyalty tier, promo × time bucket)
- Refit XGBoost on v3 feature set; evaluated AUC lift vs. v2

### Mar 17 — CatBoost + ensemble blend (final submission)
- Built independent CatBoost pipeline for ensemble diversity
- Blended XGBoost and CatBoost out-of-fold predictions (weighted average)
- Submitted final blend to private leaderboard

---

## Key Features Engineered

| Category | Features |
|----------|---------|
| **Temporal** | Hour of day, day of week, weekend flag, time bucket |
| **Payment** | Normalized payment method (Visa / MasterCard / PayPal) |
| **Customer** | Historical return rate, loyalty tier, account age |
| **Product** | Category, subcategory, price, discount depth |
| **Behavioral** | Promo code, marketing channel, review score, interaction terms |

---

## Stack

| Tool | Purpose |
|------|---------|
| R / dplyr | Data manipulation and feature engineering |
| xgboost | Primary gradient boosting model |
| catboost | Ensemble diversity |
| glmnet | LASSO baseline |
| randomForest | Random forest baseline |
| caret | Cross-validation framework |
| pROC | AUC evaluation |

---

## File Map

| File | Stage | Date |
|------|-------|------|
| `preprocessing.R` | Cleaning, timestamp features, dummy encoding | Mar 9 |
| `model_choice.R` | Baseline comparison — logistic, LASSO, RF, XGBoost | Mar 10 |
| `model_fold.R` | CV fold strategy | Mar 11 |
| `feature_engineering_v2.R` | Customer and product aggregates | Mar 11 |
| `xg_boost_fine.R` | XGBoost hyperparameter tuning | Mar 11 |
| `v2_verification.R` | Leakage checks and performance audit | Mar 15 |
| `feature_engineering_v3.R` | Behavioral features and interaction terms | Mar 16 |
| `modeling_v3.R` | Full pipeline v3 | Mar 16 |
| `xg_boost_v2.R` | XGBoost refit on v3 features | Mar 16 |
| `cat_boost_base.R` | CatBoost pipeline | Mar 17 |
| `cat_xg_blend.R` | Final XGBoost + CatBoost ensemble blend | Mar 17 |
