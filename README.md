🌍 GSOD-Climate-Analysis
This project explores historical climate trends using NOAA’s Global Summary of the Day (GSOD) dataset and Apache Spark for scalable, distributed data processing. It includes data collection, preprocessing, exploration, and machine learning modeling to predict climate patterns.

📦 Data Sources
Daily Weather Observations (1970–2023)
Source: NOAA GSOD Archive

Station Metadata (location, country, elevation)
Source: isd-history.csv

⚙ Preprocessing Overview
Notebook: notebooks/GSOD Data DL and Parquet.ipynb

Steps:

Downloads raw GSOD CSVs (1970–2023) — over 380,000 files (~32.7 GB).

Parses with a defined schema for consistency.

Derives new fields:

year, month, decade

Joins with station metadata for enriched spatial attributes.

Writes cleaned output to partitioned Parquet format.

🧹📊 Data Cleaing and Exploration
Notebook: notebooks/GSOD_Exploration.ipynb

Cleaning steps:

Dropped rows with missing or unrealistic values (e.g., TEMP, PRCP)

Removed placeholder values (99.99, 999.9, etc.)

📊 Data Exploration
Notebook: notebooks/GSOD_Exploration.ipynb

Highlights:

Yearly and decadal trends in global temperature

Station coverage across decades

Missing data analysis

Visualizations of rainfall, pressure, and wind

Interactive maps of weather stations

📈 Milestone 3: Modeling & Evaluation
Notebook: notebooks/GSOD_Modeling.ipynb

✅ Objective
To predict monthly average temperature using geographic, atmospheric, and temporal features.

✅ Features Used
Location: LATITUDE, LONGITUDE, ELEVATION

Time: year, month

Weather: PRCP (precipitation), SLP (sea-level pressure), WDSP (wind speed)

✅ Model Details
Algorithm: Gradient-Boosted Tree Regressor (PySpark MLlib)

Train RMSE: ~4.24

Test RMSE (2011–2020): ~4.41

Target Variable: AVG_TEMP

Month	Actual Avg Temp (°F)	Predicted Avg Temp (°F)
Jan	39.28	39.22
Jul	71.68	70.77
Dec	42.00	41.99

✅ Evaluation
Model fits well (low RMSE).

Minimal overfitting — training and test scores are close.

Model captures seasonal trends but slightly underestimates summer highs.

✅ Next Steps
Try RandomForestRegressor for interpretability.

Add lagged features to improve sequential predictions.

Train regional or country-specific models.

Perform residual analysis to detect systematic errors.


# GSOD-Climate-Analysis

# Test push to verify saved GitHub token
>>>>>>> b1d3eff (Test commit to verify token storage)
