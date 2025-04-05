# Cisco-Forecast-Pipeline
Forecasting Pipeline with Cost-Weighted Ensemble Optimization
Overview
This project implements a robust forecasting pipeline combining data-driven models and team-generated forecasts. It leverages multi-quarter backtesting and cost-weighted optimization to produce calibrated forecasts. The final forecast is a weighted blend of a machine-generated prediction (Our_Forecast) and a bias-corrected team ensemble, with the blend weight (α) optimized to minimize cost-weighted MAPE.

Pipeline Modules
1. Data Preprocessing (data_preprocessing.py)
Input: Excel file with:

Main Forecast Data: Product actuals (FY22 Q2–FY25 Q1), team forecasts for FY25 Q2, and Cost Rank.

Accuracy Data: Historical team performance (accuracy and bias).

Actions:

Normalize and reshape data.

Generate:

main_forecast_data.csv

accuracy_data.csv

time_series_data.csv

team_performance.csv

2. Feature Engineering (feature_engineering.py)
Input: time_series_data.csv

Actions:

Create time-series features (e.g., Lag_1, MA_3).

Perform causal discovery using Tigramite’s PCMCI.

Output: time_series_features.csv

3. Model Training (model_training.py)
Input: time_series_features.csv

Models Used:

Exponential Smoothing (with fallback to Holt’s linear)

ARIMA

XGBoost

Output:

Base forecasts combined via simple average → Our_Forecast

Saved as our_forecasts.csv

4. Final Ensemble Forecasting (final_ensemble.py)
Modified Module

Input:

our_forecasts.csv

main_forecast_data.csv

team_performance.csv

Key Features:

Cost Factor: High-value products weighted more heavily.

Global Weights: Derived using cost-weighted accuracy.

Bias Correction: Adjust team forecasts using historical bias.

Team Ensemble: Weighted sum of corrected forecasts.

Backtesting:

Uses FY22 Q2–FY25 Q1 as holdouts.

α optimized in [0.1, 0.9] to minimize cost-weighted MAPE.

Final Forecast:

Final_Forecast = α × Team Ensemble + (1 - α) × Our_Forecast

Output: Final_Forecasts_FY25Q2.xlsx

Key Insights
Optimized α = 0.20: Final forecast = 20% Team Ensemble + 80% Our_Forecast.

Global Team Weights:

Demand Planners: 39.3%

Marketing Teams: 28.6%

Stat and ML: 32.1%

Forecast Accuracy:

Weighted MAPE: ~24.74%

Final forecast closely matches actuals from recent quarters.

Recommendations
The pipeline effectively calibrates forecasts in low-data scenarios.

Consider tweaking the α range or penalizing volatility if team forecasts need more weight.

The current setup favors the machine model, likely due to its superior stability and accuracy.

