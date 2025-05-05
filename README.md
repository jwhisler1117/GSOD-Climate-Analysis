GSOC Climate-Analysis

This project explores historical climate trends using **NOAA’s Global Summary of the Day (GSOD)** dataset and **Apache Spark** for scalable data processing.

---

##  Data Sources

- **Daily Weather Observations**  
  [NOAA GSOD Archive](https://www.ncei.noaa.gov/data/global-summary-of-the-day/access/)

- **Station Metadata (location, country, elevation)**  
  [isd-history.csv](https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv)

---

##  Preprocessing Overview

Notebook: [`GSOD Data DL and Parquet.ipynb`](notebooks/GSOD%20Data%20DL%20and%20Parquet.ipynb)

1. **Downloads GSOD CSVs** from 1970 to 2023 (~382,000 files, ~32.7 GB total).
2. **Parses with a defined schema** 
3. **Adds derived fields**:  
   - `year` (from `DATE`)  
   - `decade` (for decade-based analysis)
4. **Enriches data** using NOAA Stations Metadata `isd-history.csv` (country, state, elevation).
5. **Writes to Parquet**, partitioned by `year` for performance.

---


## 🔧 Data Cleaning

To ensure the quality of the dataset, we applied basic data cleaning techniques before analysis:

```python
from pyspark.sql.functions import col, regexp_replace

# 1. Drop rows with missing essential fields
essential_cols = ["TEMP", "LATITUDE", "LONGITUDE"]
df_clean = df.dropna(subset=essential_cols)

# 2. Remove invalid or unrealistic values
df_clean = df_clean.filter((col("TEMP") > -1750) & (col("TEMP") < 1750)) \
                   .filter((col("SLP") < 1100) & (col("SLP") > 800)) \
                   .filter(~col("PRCP").isin(["99.99", "999.9", "99.9", "999.0"]))

# 3. Remove duplicate observations
df_clean = df_clean.dropDuplicates(["STATION", "DATE"])

# 4. Clean special flags in MAX, MIN, and PRCP
df_clean = df_clean.withColumn("MAX", regexp_replace(col("MAX"), "[*]", "").cast("double"))
df_clean = df_clean.withColumn("MIN", regexp_replace(col("MIN"), "[*]", "").cast("double"))
df_clean = df_clean.withColumn("PRCP", regexp_replace(col("PRCP"), "[A-Z]", "").cast("double"))

---

##  Data Exploration

Notebook: [`GSOD_Exploration.ipynb`](notebooks/GSOD_Exploration.ipynb)

Includes:
- Schema and record overview
- Summary stats and distributions
- Missing value analysis
- Temperature trends by year and decade
- Avg temperature by country
- Map of weather station locations (1970 vs 2023)


---

##  Setup Instructions

To run in Colab or a new environment:

```python
# Install core dependencies
!pip install pyspark matplotlib --quiet

# Download station metadata
!mkdir -p data
!wget https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv -O data/isd-history.csv

# GSOD-Climate-Analysis

