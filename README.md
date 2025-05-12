# End-to-End ML Pipeline for Bore-Oil Forecasting

This repository implements a complete workflow for predicting daily bore-oil production volume:

1. **Exploratory Data Analysis & ETL** (`notebooks/01_eda_etl.ipynb`)
   - Loads raw data (`data/raw/volve_prod.xlsx`), excludes bore-wells 4 & 5
   - Restores index to a `DATEPRD` column and parses dates
   - Cleans missing values, normalizes metadata
   - Visualizes distributions, correlations, and seasonal trends
   - Outputs cleaned CSV (`data/processed/production_cleaned.csv`)

2. **Feature Engineering & Model Training** (`src/feature_engineering_and_model_training.py`)
   - Extracts temporal features (month, day-of-year, day-of-week)
   - Drops identifiers/text columns
   - Builds preprocessing pipelines (median impute + scale for numerics, most-freq impute + one-hot for categoricals)
   - Grid-searches four regressors: Ridge, Lasso, Random Forest, Gradient Boosting
   - Evaluates on hold-out test set using MSE & R² metrics

## Results Summary

| Model               | CV MSE   | Test MSE | Test R² |
|---------------------|---------:|---------:|--------:|
| Ridge               | ~3960    | ~3960    | 0.998   |
| Lasso               | ~3960    | ~3960    | 0.998   |
| Random Forest       | ~1770    | ~1770    | 0.999   |
| Gradient Boosting   | ~1745    | ~1767    | 0.999   |

Tree-based methods capture non-linear patterns and halve the error of linear models, achieving near-perfect explained variance.

## Getting Started

```bash
git clone git@github.com:d-db/ml_pipeline_portfolio.git
cd ml_pipeline_portfolio
pip install -r requirements.txt

# Launch EDA notebook
jupyter lab notebooks/01_eda_etl.ipynb

# Run feature engineering & modeling
python src/feature_engineering_and_model_training.py
cat << 'EOF' > README.md
# End-to-End ML Pipeline for Bore-Oil Forecasting

This repository implements a complete workflow for predicting daily bore-oil production volume:

1. **Exploratory Data Analysis & ETL** (`notebooks/01_eda_etl.ipynb`)
   - Loads raw data (`data/raw/volve_prod.xlsx`), excludes bore-wells 4 & 5
   - Restores index to a `DATEPRD` column and parses dates
   - Cleans missing values, normalizes metadata
   - Visualizes distributions, correlations, and seasonal trends
   - Outputs cleaned CSV (`data/processed/production_cleaned.csv`)

2. **Feature Engineering & Model Training** (`src/feature_engineering_and_model_training.py`)
   - Extracts temporal features (month, day-of-year, day-of-week)
   - Drops identifiers/text columns
   - Builds preprocessing pipelines (median impute + scale for numerics, most-freq impute + one-hot for categoricals)
   - Grid-searches four regressors: Ridge, Lasso, Random Forest, Gradient Boosting
   - Evaluates on hold-out test set using MSE & R² metrics

## Results Summary

| Model               | CV MSE   | Test MSE | Test R² |
|---------------------|---------:|---------:|--------:|
| Ridge               | ~3960    | ~3960    | 0.998   |
| Lasso               | ~3960    | ~3960    | 0.998   |
| Random Forest       | ~1770    | ~1770    | 0.999   |
| Gradient Boosting   | ~1745    | ~1767    | 0.999   |

Tree-based methods capture non-linear patterns and halve the error of linear models, achieving near-perfect explained variance.
