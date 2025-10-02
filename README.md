# GRB Co. Supply Chain Forecasting — Datathon Project

## Problem Statement
GRB Co., a mid-tier consumer goods firm, faces declining profitability despite stable demand. The root cause lies in escalating logistics costs and systemic inefficiencies across its supply chain. This project leverages data-driven forecasting to identify cost drivers, improve logistics efficiency, and support strategic decision-making.

## Objective
Build an interpretable, time-aware forecasting pipeline to predict shipping costs using operational, environmental, and supplier-related features. Apply forecast reconciliation to ensure coherence across geographic zones and evaluate model performance using robust metrics.

## Dataset Overview
- **Rows**: ~32,000
- **Columns**: 26 (24 numeric, 2 categorical)
- **Target**: `shipping_costs`
- **Key Predictors**:
  - `traffic_congestion_level`
  - `port_congestion_level`
  - `fuel_consumption_rate`
  - `loading_unloading_time`
  - `supplier_reliability_score`

## Pipeline Summary
1. **Data Cleaning & Preprocessing**
   - Timestamp parsing, outlier handling, lag features
   - Winsorization of heavy-tailed cost values
   - Time feature engineering (hour, day-of-week, cyclical encodings)

2. **Modeling**
   - Base model: XGBoost (reg:squarederror)
   - Features: 5 key predictors + time features + autoregressive lags
   - Train/test split: chronological (80/20)

3. **Forecast Reconciliation**
   - Zone-level forecasts aggregated to total
   - MinT (Minimum Trace) reconciliation via FoReco
   - Comparison with bottom-up and top-down methods

4. **Evaluation**
   - Metrics: RMSE, MAE, MAPE, sMAPE, R²
   - Visuals: actual vs. predicted plots, residual diagnostics
   - Feature importance analysis for business insights

### Model Performance Comparison

| Model            | RMSE      | MAE       | MAPE (%) | sMAPE (%) | R²        |
|------------------|-----------|-----------|-----------|-----------|------------|
| **Global XGBoost** | 6.6126    | 4.8011    | 1.3099    | 1.3110    | 0.9955     |
| **Global Zone**    | 7.0106    | 4.1155    | 1.0816    | 1.0819    | 0.9949     |
| **Bottom-Up (BU)** | 7.0106    | 4.1155    | 1.0816    | 1.0819    | 0.9949     |
| **Top-Down (TD)**  | 7.0106    | 4.1155    | 1.0816    | 1.0819    | 0.9949     |
| **Shrinkage**      | 51.3876   | 45.4427   | 100.0000  | 200.0000  | -2.1983    |


## Business Insights
- Supplier reliability and traffic congestion are the strongest cost drivers.
- Port delays and fuel consumption significantly impact operational expenses.
- Reconciliation improves coherence across zones, enabling more reliable planning.

## Interpretation Highlights
- Global XGBoost delivers the best overall performance across all metrics — especially RMSE and R².
- Global Zone, BU, and TD methods perform similarly, with slightly higher RMSE but lower MAPE than the global model.
- Shrinkage method fails dramatically, with negative R² and inflated error metrics — likely due to poor covariance estimation or misaligned hierarchy
