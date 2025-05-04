# GSOD-Claimate-Analysis
This project analyzes historical weather data from NOAA's Global Summary of the Day (GSOD) dataset using Apache Spark. The goal is to explore climate trends across decades and regions using scalable, reproducible workflows.
---

## 📥 Data Sources

- **Weather observations**:  
  [NOAA GSOD Archive](https://www.ncei.noaa.gov/data/global-summary-of-the-day/access/)
- **Station metadata** (location, country, elevation):  
  [isd-history.csv](https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv)

---

## 🔧 Preprocessing

1. **Downloaded all NOAA GSOD CSVs (1970–2023)** using a custom Python script.
2. **Parsed each file** with a defined Spark schema to avoid misinterpreted types.
3. **Added derived columns**:
   - `year` from `DATE`
   - `decade` for grouped trend analysis
4. **Joined with station metadata** from `isd-history.csv` to add:
   - Country (`CTRY`)
   - State (`STATE`)
   - Latitude & Longitude (`LAT`, `LON`)
   - Elevation
5. **Converted CSVs to Apache Parquet**, partitioned by `year`, for compressed and efficient access.

📂 Output path: `/home/jovyan/Testing/gsod_parquet_enriched`

👉 **Preprocessing Notebook**:  
[`notebooks/gsod_parquet_conversion.ipynb`](notebooks/gsod_parquet_conversion.ipynb)

---

## 📊 Data Exploration

Exploratory analysis includes:

- Distribution plots for `TEMP`, `PRCP`, `WDSP`
- Missing value summary
- Average temperatures over time
- Climate trends by region and decade
- Station-level metadata mapping

👉 **Exploration Notebook**:  
[`notebooks/gsod_exploration.ipynb`](notebooks/gsod_exploration.ipynb)

---

## ⚙️ Setup Instructions (for Colab or local)

At the top of each notebook, run:

```python
# Install necessary libraries
!pip install pyspark findspark seaborn folium --quiet

# Download station metadata (one-time)
!mkdir -p data
!wget https://www.ncei.noaa.gov/pub/data/noaa/isd-history.csv -O data/isd-history.csv
