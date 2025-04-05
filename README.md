# 📈 Forecasting Pipeline with Cost-Weighted Predictions

A forecasting pipeline that blends team insights with data-driven predictions, optimized using cost-weighted error metrics and multi-quarter backtesting. The final forecast is a smart blend of machine learning and human intuition — tuned to minimize business impact.

---

## 🔧 Pipeline Overview

### 1. Data Preprocessing

**Input**: `Cisco Forecast League Clean.xlsx` (Excel file with two sheets)
- **Main Forecast Data**: Product actuals (FY22 Q2–FY25 Q1), team forecasts for FY25 Q2, and cost ranks.
- **Accuracy Data**: Historical team accuracy and bias (FY24 Q3–FY25 Q1).

**Actions**:
- Normalize column names, convert % values.
- Reshape actuals into long format.
- Compute team performance metrics.

**Output**:
- `main_forecast_data.csv`
- `accuracy_data.csv`
- `time_series_data.csv`
- `team_performance.csv`

---

### 2. Feature Engineering

**Input**: `time_series_data.csv`

**Actions**:
- Generate time-series features (e.g., `Lag_1`, `MA_3`)
- Run causal discovery (PCMCI from Tigramite) as a diagnostic

**Output**: `time_series_features.csv`

---

### 3. Model Training

**Input**: `time_series_features.csv`

**Models Used**:
- Exponential Smoothing (with Holt fallback)
- ARIMA
- XGBoost

**Output**:
- Generate base forecasts per product
- Combine via simple average → `Our_Forecast`
- Save as `our_forecasts.csv`

---

### 4. Final Ensemble Forecasting

**Input**:
- `our_forecasts.csv`
- `main_forecast_data.csv`
- `team_performance.csv`

**Key Steps**:
- Cost Factor: Prioritize high-value products based on cost rank
- Global Weights: Cost-weighted accuracy determines team influence
- Bias Correction: Adjust team forecasts using historical bias
- Team Ensemble: Weighted sum of corrected forecasts
- Backtesting: Optimize α ∈ [0.1, 0.9] over holdout quarters (FY22 Q2–FY25 Q1)
- Final Blend:  
  `Final_Forecast = α × Team Ensemble + (1 - α) × Our_Forecast`

**Output**: `Final_Forecasts_FY25Q2.xlsx`

---

## 📌 Key Results

- Optimal α = `0.20` → Final forecast = 20% Team Ensemble + 80% Our_Forecast
- Global Team Weights:
  - Demand Planners: 39.3%
  - Marketing Teams: 28.6%
  - Stat/ML: 32.1%
- Weighted MAPE ≈ 24.74%
- Final forecast aligns closely with actual holdout values

---

## 💬 Interpretation

- A low α indicates the machine model outperformed team forecasts during backtesting
- Bias correction and cost-based weights ensure team inputs are appropriately valued
- The pipeline is flexible — you can adjust α range or increase team influence if needed

---

## 🚀 Running the Pipeline

To run the full forecasting pipeline:

1. Make sure the input file `Cisco Forecast League Clean.xlsx` is present in the same directory
2. Open the Jupyter notebook `Cisco-Forecast-Pipeline.ipynb`
3. Run all cells from top to bottom


The final forecast will be saved as:

**Output**: `Final_Forecasts_FY25Q2.xlsx`

---

## 🧠 Final Thoughts

This pipeline is designed to deliver calibrated forecasts in data-scarce environments.  
By blending algorithmic stability with domain expertise, it reduces forecasting error — especially where cost sensitivity matters.

