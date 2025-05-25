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

## 📈 Milestone 3: Modeling & Evaluation

Notebook: [`notebooks/GSOD_Modeling.ipynb`](notebooks/GSOD_Modeling.ipynb)

### ✅ Objective

To predict monthly average temperature using:

- **📍 Location**: `LATITUDE`, `LONGITUDE`, `ELEVATION`  
- **📅 Time**: `year`, `month`  
- **🌦️ Weather**: `PRCP`, `SLP`, `WDSP`

---

### ✅ Preprocessing Summary

- Cleaned weather data and filtered stations with at least 45 years of data
- Built feature vectors using PySpark’s `VectorAssembler`
- Trained on data from 1970–2009  
- Tested on data from 2011–2020

---

### ✅ Model Details

- **Algorithm**: Gradient-Boosted Tree Regressor (Spark MLlib)
- **Training RMSE**: ~4.24
- **Testing RMSE**: ~4.41

| Month | Actual Temp (°F) | Predicted Temp (°F) |
|-------|------------------|---------------------|
| Jan   | 39.28            | 39.22              |
| Jul   | 71.68            | 70.77              |
| Dec   | 42.00            | 41.99              |

---

### ✅ Evaluation

- 🔹 RMSE shows low error and good generalization
- 🔹 Model captures seasonal trends
- 🔹 Slight underestimation of summer highs

---

### 🔮 Next Steps

- Try `RandomForestRegressor` for easier interpretation
- Add **lagged features** for monthly dependencies
- Train **regional models** for better accuracy
- Use residual analysis to identify outliers

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
