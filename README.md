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

=======
This project explores historical climate trends using NOAA’s Global Summary of the Day (GSOD) dataset and Apache Spark for scalable data processing.

📦 Data Sources
Daily Weather Observations
NOAA GSOD Archive

Station Metadata (location, country, elevation)
isd-history.csv

⚙ Preprocessing Overview
Notebook: GSOD Data DL and Parquet.ipynb

Downloads raw GSOD CSVs (1970–2023).

Parses data with defined schema.

Adds derived fields: year, decade.

Joins with station metadata (isd-history.csv) for enriched spatial information.

Writes data to partitioned Parquet format for efficient querying.

🧹 Data Cleaning
Notebook: GSOD_Exploration.ipynb

Cleaning steps included:

Dropping rows with missing or placeholder values (TEMP, LATITUDE, PRCP)

Filtering out unrealistic values

Removing special characters (e.g. * in MAX, MIN)

Casting string-encoded numbers to proper numeric types

Deduplication on STATION and DATE

📊 Data Exploration
We examined:

Missing data distribution

Long-term temperature trends

Station location evolution

Global averages per year and decade

Rainfall and pressure patterns

📈 Milestone 3: Modeling & Evaluation
Notebook: GSOD_Modeling.ipynb

✅ Model Goal
To predict monthly average temperature using:

📍 Location: LATITUDE, LONGITUDE, ELEVATION

📅 Time: year, month

🌦️ Weather: PRCP, SLP, WDSP

✅ Preprocessing (Required by Milestone)
Data filtered by quality (e.g. invalid TEMP or PRCP values removed)

Feature selection and scaling were handled via Spark VectorAssembler

Month was expanded into name for visualization, no advanced encoding needed

✅ Model Details
Model Type: Gradient-Boosted Tree Regressor (Spark MLlib)

Training RMSE: ~4.24

Test RMSE (2011–2020): ~4.41

Target: AVG_TEMP by month

Month	Actual Temp (°F)	Predicted Temp (°F)
Jan	39.28	39.22
Jul	71.68	70.77
Dec	42.00	41.99

✅ Evaluation: Fitting Graph Position
Our model fits moderately well — low error, but not overfitting.

It generalizes well from training to unseen data.

Visual inspection showed the model is able to capture seasonality but tends to slightly underestimate summer highs.

✅ Next Steps / Future Models
🧠 Try RandomForestRegressor for interpretability

🕸️ Add lagged features to capture month-to-month dynamics

🗺️ Train per-country or per-region models

📉 Use residual analysis to identify months with worst error

