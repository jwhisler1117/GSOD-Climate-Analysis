# GSOD-Climate-Analysis

This project explores historical climate trends using NOAA’s **Global Summary of the Day (GSOD)** dataset and **Apache Spark** for scalable, distributed data processing. It includes data collection, cleaning, exploration, and machine learning modeling to predict temperature trends.

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
- **Training RMSE**: ~4.24°F  
- **Testing RMSE**: ~4.41°F

#### Sample Predictions

| Month | Actual Avg Temp (°F) | Predicted Avg Temp (°F) |
|-------|----------------------|--------------------------|
| Jan   | 39.28                | 39.22                   |
| Jul   | 71.68                | 70.77                   |
| Dec   | 42.00                | 41.99                   |

---

### Evaluation

- RMSE shows strong generalization and low overfitting
- Captures seasonal patterns well
- Slight underestimation in peak summer months

---

### Next Steps

- Try `RandomForestRegressor` for interpretability
- Add **lagged features** to capture temporal patterns
- Train **region-specific models**
- Conduct **residual analysis** to detect outliers

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


