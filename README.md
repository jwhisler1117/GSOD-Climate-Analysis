🌍 GSOD-Climate-Analysis
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
>>>>>>> b1d3eff (Test commit to verify token storage)
