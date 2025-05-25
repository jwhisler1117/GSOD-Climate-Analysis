# 🌍 GSOD-Climate-Analysis

This project explores historical climate trends using NOAA’s **Global Summary of the Day (GSOD)** dataset and **Apache Spark** for scalable, distributed data processing. It includes data collection, cleaning, exploration, and machine learning modeling to predict temperature trends.

---

## 📦 Data Sources

- **Daily Weather Observations (1970–2023)**  
  [NOAA GSOD Archive](https://www.ncei.noaa.gov/data/global-summary-of-the-day/access/)

- **Station Metadata** (location, country, elevation)  
  [ISD History CSV](https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv)

---

## ⚙ Preprocessing Overview

Notebook: [`notebooks/GSOD Data DL and Parquet.ipynb`](notebooks/GSOD%20Data%20DL%20and%20Parquet.ipynb)

Steps:

- Downloads 380,000+ raw GSOD CSV files (32+ GB)
- Parses with a defined schema for consistency
- Adds derived columns: `year`, `month`, `decade`
- Joins with station metadata for spatial enrichment
- Saves cleaned output as partitioned Parquet files

---

## 🧹 Data Cleaning & 📊 Exploration

Notebook: [`notebooks/GSOD_Exploration.ipynb`](notebooks/GSOD_Exploration.ipynb)

### Cleaning Steps:

- Dropped rows with missing or placeholder values (`TEMP`, `PRCP`, etc.)
- Filtered out unrealistic entries (e.g., `TEMP < -1750`)
- Removed special characters and cleaned numeric fields
- Deduplicated records by `STATION` and `DATE`

### Exploration Highlights:

- Global temperature trends (yearly and decadal)
- Seasonal and geographic variation
- Missing data patterns
- Interactive weather station maps

---

##  Modeling & Evaluation

Notebook: [`notebooks/GSOD_Modeling.ipynb`](notebooks/GSOD_Modeling.ipynb)

### ✅ Objective

To predict the average monthly temperature from the 2011-2020 decade based on data from 1970-2010 using these features:

- **Location**: `LATITUDE`, `LONGITUDE`, `ELEVATION`  
- **Time**: `year`, `month`  
- **Weather**: `PRCP`, `SLP`, `WDSP`

---

### ✅ Preprocessing Summary

- Missing values removed for key features: TEMP, PRCP, SLP, WDSP, LATITUDE, LONGITUDE, ELEVATION
- Invalid placeholder values like 99.99, 9999 filtered out
- Features assembled using PySpark’s VectorAssembler:
  - LATITUDE, LONGITUDE, ELEVATION
  - year, month (temporal)
  - PRCP, SLP, WDSP (atmospheric)

---

### Model Details

- **Algorithm**: Gradient-Boosted Tree Regressor (Spark MLlib)
- **Target**: Monthly Average Temperature (AVG_TEMP)
- **Training Data:** 1970-2009
- **Test Data:** 2011-2020

### Model Evaluation

**Metric	Value**
  Train RMSE	4.24°F
  Test RMSE	  4.41°F

**Month	Actual Temp (°F)	Predicted Temp (°F)**
  Jan	  39.28            	39.22
  Jul	  71.68	            70.77
  Dec	  42.00	            41.99
---

### Evaluation

**Fitting Position:** Fairly Strong (low RMSE difference between train and test suggesting minimal overfitting)

**Future Plans:**
  Add lagged features to capture temporal dependencies
  Train per-country or per-region models
  Try RandomForestRegressor for improved interpretability
  Perform residual analysis to target months with higher error

---

### Conclusion

-The GBT model effectively predicts monthly temperature using only station, time, and weather data.
-It generalizes well from historical to modern climate data.
-Future improvements include regional modeling and time-series enhancements like lag features
---

## 📁 Notebooks

- 📄 `GSOD Data DL and Parquet.ipynb` – Downloads and prepares the data  
- 📄 `GSOD_Exploration.ipynb` – Cleans and visualizes the dataset  
- 📄 `GSOD_Modeling.ipynb` – Machine learning pipeline and evaluation  

---

## 🚀 Setup

To run locally or in Jupyter:

```bash
pip install pyspark matplotlib
