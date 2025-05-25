🌍 GSOD-Climate-Analysis
This project explores historical climate trends using NOAA’s Global Summary of the Day (GSOD) dataset and Apache Spark for scalable, distributed data processing. It includes data collection, preprocessing, exploration, and machine learning modeling to predict temperature trends.

📦 Data Sources
Daily Weather Observations (1970–2023)
NOAA GSOD Archive

Station Metadata (location, country, elevation)
ISD History CSV

⚙ Preprocessing Overview
Notebook: notebooks/GSOD Data DL and Parquet.ipynb

Steps:

Downloads raw GSOD CSVs (1970–2023) – 380,000+ files (~32.7 GB)

Parses with a defined schema

Adds derived columns: year, month, decade

Joins with station metadata for spatial enrichment

Saves partitioned Parquet files by year

🧹 Data Cleaning & 📊 Exploration
Notebook: notebooks/GSOD_Exploration.ipynb

Cleaning Highlights:
Removed missing or placeholder values (TEMP, PRCP, etc.)

Filtered unrealistic values (e.g., TEMP < -1750, PRCP == 99.99)

Removed special flags and cast numeric fields

Deduplicated by STATION and DATE

Exploration Highlights:
Global station distribution maps

Temperature trends by year and decade

Seasonal and regional variation analysis

Rainfall and pressure patterns

📈 Milestone 3: Modeling & Evaluation
Notebook: notebooks/GSOD_Modeling.ipynb

✅ Objective
Predict monthly average temperature using:

🌍 Location: LATITUDE, LONGITUDE, ELEVATION

📅 Time: year, month

🌦️ Weather: PRCP, SLP, WDSP

✅ Preprocessing
Cleaned weather data and filtered for complete stations

Used PySpark’s VectorAssembler to build feature vectors

Split data into training (before 2010) and test (2011–2020)

✅ Model Details
Algorithm: Gradient-Boosted Tree Regressor (PySpark MLlib)

Train RMSE: ~4.24

Test RMSE (2011–2020): ~4.41

Month	Actual Avg Temp (°F)	Predicted Avg Temp (°F)
Jan	39.28	39.22
Jul	71.68	70.77
Dec	42.00	41.99

✅ Evaluation
Good generalization: training and test RMSE are close

Seasonal patterns captured well

Slight underestimation of summer highs

🔮 Next Steps
Try RandomForestRegressor for interpretability

Add lag features (e.g. previous month’s weather)

Train region-specific models

Perform residual analysis to find outliers
