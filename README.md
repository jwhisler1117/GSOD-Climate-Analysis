# 🌍 GSOD-Climate-Analysis

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
2. **Parses with a defined schema** for reliable typing.
3. **Adds derived fields**:  
   - `year` (from `DATE`)  
   - `decade` (for decade-based analysis)
4. **Enriches data** using `isd-history.csv` (country, state, lat/lon, elevation).
5. **Writes to Parquet**, partitioned by `year` for performance.

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
- Precipitation and wind patterns (optional)

---

##  Setup Instructions

To run in Colab or a new environment:

```python
# Install core dependencies
!pip install pyspark findspark seaborn folium matplotlib --quiet

# Download station metadata
!mkdir -p data
!wget https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv -O data/isd-history.csv

