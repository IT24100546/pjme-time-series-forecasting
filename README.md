# PJM Hourly Energy Consumption - Time Series Forecasting

End-to-end time series forecasting project using hourly electricity consumption data (PJME region, in megawatts) from PJM Interconnection.

## Goal

Forecast future electricity demand and compare multiple forecasting models to find which one performs best on seasonal, real-world data.

## Dataset

- **Source:** [PJM Hourly Energy Consumption (Kaggle)](https://www.kaggle.com/datasets/robikscube/hourly-energy-consumption)
- **File used:** `PJME_hourly.csv`
- ~145,000 hourly records (2002-2018), resampled to daily averages (~6,000 rows) for modeling

## Tools

Python, pandas, numpy, matplotlib, statsmodels, scikit-learn, Prophet

## Workflow

1. Load and clean the data (datetime parsing, duplicate check, missing value check)
2. Resample from hourly to daily averages
3. Visualize the series and zoom into a single year to inspect patterns
4. Decompose the series into trend, seasonality, and residual
5. Check stationarity with the Augmented Dickey-Fuller test
6. Inspect ACF/PACF plots to guide ARIMA parameter choice
7. Split into train/test sets (last 90 days held out as test)
8. Build and evaluate 5 forecasting models:
   - Moving Average (baseline)
   - Linear Regression (date-based features)
   - ARIMA
   - SARIMA (weekly seasonality)
   - Prophet
9. Compare models using MAE, RMSE, and MAPE

## Results

| Model | MAE | RMSE | MAPE |
|---|---|---|---|
| **Prophet** | 2608.69 | 3367.85 | **7.84%** |
| Linear Regression | 4250.21 | 5180.29 | 12.62% |
| SARIMA | 5652.25 | 7135.03 | 15.75% |
| Moving Average | 5741.52 | 7356.41 | 15.91% |
| ARIMA | 6077.12 | 7707.94 | 16.83% |

## Conclusion

Prophet performed the best out of all five models, with the lowest error across MAE, RMSE, and MAPE. It was the only model that captured both the weekly pattern and the rising trend going into summer at the same time. SARIMA correctly picked up the weekly seasonality but missed the yearly trend since it was only set up to look at a 7-day cycle. ARIMA performed the worst, even worse than the moving average baseline, because with no seasonal component its 90-day forecast flattens out toward the average instead of following the trend. Linear Regression landed in the middle, but its sawtooth-shaped predictions show it wasn't learning a meaningful pattern from the day-of-week feature.

For data with multiple layers of seasonality (daily, weekly, yearly), a model like Prophet — which handles all of that automatically — works better than models that need the seasonal period manually specified.

## How to Run

1. Download `PJME_hourly.csv` from the Kaggle link above
2. Open `PJME_Time_Series_Forecasting.ipynb` in Google Colab
3. Upload the CSV to the Colab session
4. Run all cells in order
