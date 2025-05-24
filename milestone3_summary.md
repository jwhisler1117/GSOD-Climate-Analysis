# 🧠 Milestone 3 Summary

## ✅ Model Objective
Predict the average monthly temperature using global weather station data enriched with geographic and atmospheric features.

## 📌 Dataset Summary
- **Source:** GSOD (Global Summary of the Day)
- **Period:** 1970–2023
- **Filtering:** Only stations with ≥ 45 years of complete data were used.

## 🧹 Preprocessing
- Dropped nulls in essential columns (`TEMP`, `LATITUDE`, `LONGITUDE`, etc.)
- Removed unrealistic values and known flag codes from `PRCP`, `SLP`, `WDSP`
- Aggregated daily data to monthly averages

## 🧪 Features Used
- Latitude
- Longitude
- Elevation
- Year
- Month
- Average monthly precipitation (PRCP)
- Sea-level pressure (SLP)
- Wind speed (WDSP)

## 📈 Model Details
- **Model Type:** Gradient-Boosted Tree Regressor
- **Tool:** PySpark MLlib
- **Train/Test Split:** 80/20
- **Train RMSE:** 4.37
- **Test RMSE:** 4.38

## 🔍 Evaluation
The model generalizes well and shows no significant overfitting. Residuals are fairly balanced across months and locations.

## 📉 Where does this fit on the underfitting/overfitting curve?
> The model is in the **“just right”** region. Train and test RMSE are very close, indicating healthy generalization.

## 🔮 Next Steps
- Experiment with Random Forest and Linear Regression for comparison
- Add lag features (previous month’s temperature)
- Try per-region models or cluster stations
- Add target variable for PRCP (rainfall) prediction

## 📎 Files
- [`notebooks/GSOD_Modeling.ipynb`](notebooks/GSOD_Modeling.ipynb)

---X
