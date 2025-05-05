# 🌍 GSOD-Climate-Analysis

This project explores historical climate trends using **NOAA’s Global Summary of the Day (GSOD)** dataset and **Apache Spark** for scalable data processing.

---

## 📦 Data Sources

- **Daily Weather Observations**  
  [NOAA GSOD Archive](https://www.ncei.noaa.gov/data/global-summary-of-the-day/access/)

- **Station Metadata (location, country, elevation)**  
  [isd-history.csv](https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv)

---

## ⚙ Preprocessing Overview

Notebook: [`GSOD Data DL and Parquet.ipynb`](notebooks/GSOD%20Data%20DL%20and%20Parquet.ipynb)

1. **Downloads GSOD CSVs** from 1970 to 2023 (~382,000 files, ~32.7 GB total).
2. **Parses with a defined schema** for consistent data types.
3. **Adds derived fields**:
   - `year` (from `DATE`)
   - `decade` (for decade-based trend analysis)
4. **Enriches data** using `isd-history.csv` to include:
   - Country, State, Latitude, Longitude, and Elevation
5. **Writes to Parquet**, partitioned by `year` for efficient querying.

---

## 🧹 Data Cleaning

To ensure data quality, the following cleaning steps were applied before analysis:

- 🔸 Drop rows with missing essential fields (`TEMP`, `LATITUDE`, `LONGITUDE`)
- 🔸 Filter out invalid or unrealistic values (e.g., `TEMP < -1750` or placeholder `PRCP` values like `"99.99"`)
- 🔸 Remove duplicate records based on `STATION` and `DATE`
- 🔸 Clean special flags:
  - Remove asterisks (`*`) from `MAX`/`MIN`
  - Strip alpha flags from `PRCP` (e.g., `"0.00A"` becomes `0.00`)

---

## 📊 Data Exploration

Notebook: [`GSOD_Exploration.ipynb`](notebooks/GSOD_Exploration.ipynb)

Key insights include:

- Schema and record overview
- Summary statistics and distributions for temperature, wind, and precipitation
- Missing data analysis
- Temperature trends over years and decades
- Temperature variation by country
- Maps comparing weather station locations from 1970 vs 2023

---

## 🚀 Setup Instructions

To run in Colab or a local environment:

```python
# Install core dependencies
!pip install pyspark matplotlib --quiet

# Download station metadata
!mkdir -p data
!wget https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv -O data/isd-history.csv
