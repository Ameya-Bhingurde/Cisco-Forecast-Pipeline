📈 Forecasting Pipeline with Cost-Weighted Ensemble Optimization
A forecasting pipeline that blends team insights with data-driven predictions, optimized using cost-weighted error metrics and multi-quarter backtesting. The final forecast is a smart blend of machine learning and human intuition — tuned to minimize business impact. 💡

🔧 Pipeline Overview
1️⃣ Data Preprocessing
📥 Input: Cisco Forecast League Clean.xlsx
Includes two sheets:

Main Forecast Data: Product actuals (FY22 Q2–FY25 Q1), team forecasts for FY25 Q2, and cost ranks.

Accuracy Data: Historical team accuracy and bias (FY24 Q3–FY25 Q1).

⚙️ Actions:

Normalize column names, convert % values.

Reshape actuals into long format.

Compute team performance metrics.

📤 Output:

main_forecast_data.csv

accuracy_data.csv

time_series_data.csv

team_performance.csv

2️⃣ Feature Engineering
📥 Input: time_series_data.csv

⚙️ Actions:

Generate time-series features (e.g., Lag_1, MA_3)

Run causal discovery (PCMCI from Tigramite) as a diagnostic

📤 Output: time_series_features.csv

3️⃣ Model Training
📥 Input: time_series_features.csv

🤖 Models Used:

Exponential Smoothing (w/ Holt fallback)

ARIMA

XGBoost

📊 Output:

Generate base forecasts for each product

Combine via simple average → Our_Forecast

Save as our_forecasts.csv

4️⃣ Final Ensemble Forecasting
🔥 Modified Module

📥 Input:

our_forecasts.csv

main_forecast_data.csv

team_performance.csv

⚙️ Key Steps:

🎯 Cost Factor: Prioritize high-value products based on cost rank

📊 Global Weights: Cost-weighted accuracy determines team influence

🔧 Bias Correction: Adjust team forecasts using historical bias

🧠 Team Ensemble: Weighted sum of corrected forecasts

⏪ Backtesting: Optimize α ∈ [0.1, 0.9] using holdout quarters (FY22 Q2–FY25 Q1)

🧮 Final Blend:
Final_Forecast = α × Team Ensemble + (1 - α) × Our_Forecast

📤 Output: Final_Forecasts_FY25Q2.xlsx

📌 Key Results
✅ Optimal α = 0.20 → Final forecast = 20% Team Ensemble + 80% Our_Forecast

🏋️ Global Weights:

Demand Planners: 39.3%

Marketing Teams: 28.6%

Stat/ML: 32.1%

📉 Weighted MAPE ≈ 24.74%

📐 Final forecast aligns closely with actual holdout values

💬 Interpretation
Low α means the data-driven forecast is more reliable than team inputs

Bias correction and backtesting improve alignment with real demand

α range and weighting logic can be adjusted to reflect business preferences

🚀 Running the Pipeline
Simply open and run all cells in the Jupyter notebook:

bash
Copy
Edit
Cisco-Forecast-Pipeline.ipynb
📂 Ensure the input file Cisco Forecast League Clean.xlsx is in the same directory.
📄 Final forecast will be saved as:

Copy
Edit
Final_Forecasts_FY25Q2.xlsx
🧠 Final Thoughts
This pipeline is designed for robustness and flexibility in low-data environments. By blending statistical forecasting with human expertise — and fine-tuning that blend using backtesting — it offers a smart, cost-aware approach to demand prediction
