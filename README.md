# Airbnb Price Prediction — Single-Notebook Project

This repo contains a single, end‑to‑end Jupyter notebook that loads Airbnb Open Data, cleans and engineers features, explores the data, and trains baseline regression models to predict nightly price.

## What’s in here

* `airbnb_price_prediction_full_project.ipynb` — the entire workflow in one notebook

  * `features_X.csv` — cleaned numeric feature matrix
  * `target_y.csv` — cleaned price target

## Dataset

* Source: “Airbnb Open Data” (Kaggle)
* Expected filename and location: place `Airbnb_Open_Data.csv` in the project root (same folder as the notebook).
* Filepath used by the notebook: `./Airbnb_Open_Data.csv`

### 1) Install kaggle CLI
pip install kaggle

### 2) Place your Kaggle API token at: ~/.kaggle/kaggle.json
####    (Create one at https://www.kaggle.com/settings/account)
mkdir -p ~/.kaggle
chmod 600 ~/.kaggle/kaggle.json

# 3) Download and prepare the dataset
kaggle datasets download -d dgomonov/new-york-city-airbnb-open-data -p .
unzip -o new-york-city-airbnb-open-data.zip
mv -f AB_NYC_2019.csv Airbnb_Open_Data.csv


## Quickstart

```bash
# 1) Clone
git clone https://github.com/josedaniel-dev/airbnb-price-prediction.git
cd airbnb-price-prediction

# 2) (Recommended) Create a virtual environment
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

# 3) Install dependencies
pip install -r requirements.txt

# 4) Launch Jupyter and run the single notebook top-to-bottom
jupyter lab   # or: jupyter notebook
```

> Note: The notebook expects the CSV at `./Airbnb_Open_Data.csv`. If your file is elsewhere or named differently, update the path in the first data-loading cell.

## Notebook sections (the “project bible”)

1. **Data Loading & Initial Inspection**
   Loads the raw CSV, standardizes the `price` field to numeric, and prints dataset shape, column types, and missing-value overview to understand data quality and guide cleaning.

2. **Preprocessing & Feature Engineering**

   * Cleans `price` (removes symbols, coerces to numeric, filters out non-positive and extreme outliers).
   * Drops qualitative/text columns not used for the baseline model.
   * Fills `reviews_per_month` missing values with 0.
   * Optional engineered signals if present:

     * `host_days_active` from `host_since` (days since host joined)
     * `amenity_count` from `amenities` (simple count)
   * Keeps **only numeric** columns for modeling; writes `features_X.csv` and `target_y.csv` for reuse.

3. **Exploratory Data Analysis (EDA)**

   * Correlation matrix of numeric features (including `price`)
   * Top correlations with `price`
   * Scatterplots of the most price-related features
   * Price distribution and boxplot (with a sensible x‑axis cap to visualize tails)

4. **Model Building & Evaluation**

   * Train/test split (default 80/20, `random_state=42`)
   * Baselines: Linear Regression and Decision Tree Regressor
   * Metrics reported: **MAE** and **RMSE** on the test set
   * (Optional) Additional models for comparison (e.g., Random Forest, XGBoost) and simple hyperparameter search

5. **Results & Next Steps**

   * Brief comparison of model performance
   * Notes on error patterns and potential feature improvements (e.g., spatial features, distance-to-CBD, engineered review dynamics)
   * Clear to‑dos if you want to extend the project (geospatial features, external data like schools/POIs, model tuning, SHAP explainability)

## Features used (baseline)

Quantitative predictors taken from the cleaned dataset (subject to availability in the CSV):

* `lat`, `long`
* `Construction year`
* `minimum nights`
* `number of reviews`
* `reviews per month` (filled with 0 when missing)
* `review rate number`
* `calculated host listings count`
* `availability 365`
* Optional engineered: `host_days_active`, `amenity_count` (if source columns exist)

Target:

* `price` (USD, cleaned and filtered)

## Reproducibility details

* Deterministic splits with `random_state=42`.
* Saved artifacts: `features_X.csv`, `target_y.csv`.
* Python ≥ 3.9; key libraries: pandas, numpy, scikit‑learn, matplotlib, seaborn.

## Example results (will vary by environment)

* Linear Regression: MAE \~ 280–300, RMSE \~ 320–340
* Decision Tree: similar baseline range without tuning

Use the optional comparison cells to try Random Forest or XGBoost and perform quick grid searches; tree ensembles often improve MAE/RMSE with minimal extra work.

## Known quirks

* If your plotting backend lacks emoji glyphs, matplotlib may warn about missing characters in plot titles. It’s safe to ignore or you can remove emojis from titles in the notebook.

## License

MIT — feel free to fork, adapt, and build on this.

## Acknowledgments

Thanks to the creators/maintainers of the Airbnb Open Data on Kaggle and the open‑source Python ecosystem.
