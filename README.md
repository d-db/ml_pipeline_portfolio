# Bore-Oil Production Forecasting

This project implements an end-to-end machine-learning workflow for predicting daily oil-production volume from bore-well data. It consists of three main stages:

1. **Exploratory Data Analysis & ETL**  
   _Notebook: `notebooks/01_eda_etl.ipynb`_  
   - Loads raw data (`data/raw/volve_prod.xlsx`), inspects shape, types, and basic statistics  
   - Identifies high-missingness wells (4 & 5) and excludes them from modeling  
   - Resets index to preserve original `DATEPRD` column  
   - Cleans and normalizes metadata; parses dates  
   - Visualizes distributions, correlations, and seasonal trends  
   - Exports a cleaned CSV (`data/processed/production_cleaned.csv`)

2. **Feature Engineering & Preprocessing**  
   _Continued in `01_eda_etl.ipynb` and mirrored in `src/feature_engineering_and_model_training.py`_  
   - Extracts temporal features: month, day-of-year, day-of-week  
   - Drops identifier/text columns (e.g. well names, field codes)  
   - Builds a `ColumnTransformer` with:  
     - **Numeric pipeline**: median imputation → standard scaling  
     - **Categorical pipeline**: most-frequent imputation → one-hot encoding  

3. **Model Training & Evaluation**  
   _Script: `src/feature_engineering_and_model_training.py`_  
   - Splits data into train/test (80/20, random_state=42)  
   - Runs `GridSearchCV` over four regressors:  
     - Ridge, Lasso (linear)  
     - Random Forest, Gradient Boosting (tree-based)  
   - Evaluates with MSE and R² on the hold-out test set

## Results Summary

| Model               | CV MSE   | Test MSE | Test R² |
|---------------------|---------:|---------:|--------:|
| Ridge               | ~3960    | ~3960    | 0.998   |
| Lasso               | ~3960    | ~3960    | 0.998   |
| Random Forest       | ~1770    | ~1770    | 0.999   |
| Gradient Boosting   | ~1745    | ~1767    | 0.999   |

Tree-based models capture non-linear patterns and roughly halve the error of linear models, achieving near-perfect explained variance.

## Getting Started

```bash
git clone git@github.com:d-db/ml_pipeline_portfolio.git
cd ml_pipeline_portfolio
pip install -r requirements.txt

# Launch the EDA & ETL notebook
jupyter lab notebooks/01_eda_etl.ipynb

# Or run the standalone script
python src/feature_engineering_and_model_training.py
EOD
