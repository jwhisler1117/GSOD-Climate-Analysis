# GSOD-Climate-Analysis

This project explores historical climate trends using NOAA’s **Global Summary of the Day (GSOD)** dataset and **Apache Spark** for scalable, distributed data processing. It includes data collection, cleaning, exploration, and machine learning modeling to predict temperature trends.

## 📄 Full Report
View the full project write-up with figures and detailed discussion:  
[Download PDF Report](report/GSOD_Climate_Analysis_Report.pdf)

---

## Data Sources

- **Daily Weather Observations (1970–2023)**  
  [NOAA GSOD Archive](https://www.ncei.noaa.gov/data/global-summary-of-the-day/access/)

- **Station Metadata (location, country, elevation)**  
  [ISD History CSV](https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv)

---

## Preprocessing Overview

Notebook: [`notebooks/GSOD Data DL and Parquet.ipynb`](notebooks/GSOD%20Data%20DL%20and%20Parquet.ipynb)

Steps:

- Downloads 380,000+ raw GSOD CSV files (32+ GB)
- Parses with a defined schema for consistency
- Adds derived columns: `year`, `month`, `decade`
- Joins with station metadata for spatial enrichment
- Saves cleaned output as partitioned Parquet files

---

## Data Cleaning & Exploration

Notebook: [`notebooks/GSOD_Exploration.ipynb`](notebooks/GSOD_Exploration.ipynb)

### Cleaning Steps

- Dropped rows with missing or placeholder values (`TEMP`, `PRCP`, etc.)
- Filtered out unrealistic entries (e.g., `TEMP < -1750`)


### Exploration Highlights

- Global temperature trends (yearly and decadal)
- Seasonal and geographic variation
- Missing data patterns
- Interactive weather station maps

---

## Modeling 

Notebook: [`notebooks/GSOD_Modeling.ipynb`](notebooks/GSOD_Modeling.ipynb)

### Objective

To predict the average monthly temperature from the 2011–2020 decade based on data from 1970–2010 using the following features:

- **Location**: `LATITUDE`, `LONGITUDE`, `ELEVATION`
- **Time**: `year`, `month`
- **Weather**: `PRCP`, `SLP`, `WDSP`

---

### Preprocessing Summary

- Removed missing or placeholder values in core fields (`TEMP`, `PRCP`, `SLP`, `WDSP`)
- Filtered out unrealistic values (e.g. `PRCP = 99.99`)
- Feature vectors created using PySpark’s `VectorAssembler`
- Training data: 1970–2009  
- Testing data: 2011–2020

---

### Model Details

- **Algorithm**: Gradient-Boosted Tree Regressor (Spark MLlib)
- **Target**: Monthly average temperature (`AVG_TEMP`)

### 📈 Performance Metrics

- **Train RMSE**: 3.71°F  
- **Test RMSE**: 3.82°F  
- **Test R²**: 0.97  
- **Test MAE**: 2.76°F  

The model demonstrated strong generalization with low error and accurately captured seasonal temperature patterns. Most predictions were within a few degrees of the actual values.
---

### 📅 Monthly Predictions: 2011–2020

| Month | Actual Avg Temp (°F) | Predicted Avg Temp (°F) |
|-------|----------------------|--------------------------|
| Jan   | 39.49                | 39.45                   |
| Feb   | 41.95                | 41.81                   |
| Mar   | 48.63                | 48.22                   |
| Apr   | 56.31                | 55.82                   |
| May   | 63.16                | 62.55                   |
| Jun   | 68.67                | 68.45                   |
| Jul   | 71.63                | 71.11                   |
| Aug   | 70.89                | 70.51                   |
| Sep   | 65.57                | 65.31                   |
| Oct   | 57.61                | 57.45                   |
| Nov   | 48.85                | 48.76                   |
| Dec   | 42.27                | 42.19                   |

 The model was especially accurate during winter and shoulder seasons, with minor underestimations in peak summer months.

### Evaluation

- RMSE shows strong generalization and low overfitting
- Captures seasonal patterns well
- Slight underestimation in peak summer months

---

### Next Steps

- Explore **regional or climate zone-specific models** to assess local accuracy and variability
- Investigate **why the model slightly underestimates summer peaks**, possibly by refining features or adding interaction terms
- Experiment with **true future forecasting** by creating models that simulate or estimate inputs (e.g., precipitation) beyond 2020
- Try alternative models like `RandomForestRegressor` to compare performance and feature importance
- Incorporate **time-based features** or lags to improve temporal awareness
- Continue refining the data pipeline for even more scalable or real-time applications
---

## Conclusion

This project successfully demonstrates that:

- A Gradient-Boosted Tree model can accurately predict monthly average temperatures using historical weather and location data.
- The model performed well, achieving low error on both training and testing datasets, indicating strong generalization.
- Seasonal patterns are captured effectively, though minor underestimations exist for summer months.
- The modeling pipeline provides a scalable and effective approach for historical climate trend analysis and sets the stage for future forecasting improvements.

---

## Notebooks

- `GSOD Data DL and Parquet.ipynb` – Data ingestion and preprocessing  
- `GSOD_Exploration.ipynb` – Cleaning and exploratory data analysis  
- `GSOD_Modeling.ipynb` – Feature engineering, model training, evaluation

---

## Setup

To run this project locally or in a Jupyter environment:

Install the required libraries:

```bash
pip install pyspark matplotlib folium

NOAA CSV Data:

mkdir -p data
wget https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv -O data/isd-history.csv


