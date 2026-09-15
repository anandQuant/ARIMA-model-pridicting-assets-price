# 📈 ARIMA Stock Price Forecasting

A simple project that uses the **ARIMA** model to predict future stock prices based on past price data. This example uses **NIFTY 50 5-minute price data**, but it can work with any stock's price history.

## 🤔 What is ARIMA?

ARIMA stands for **AutoRegressive Integrated Moving Average**. In simple words, it's a statistical model that looks at how a price has moved in the past and uses that pattern to guess where it might go next.

## 📂 What This Project Does (Step by Step)

1. **Loads the data** – Reads historical price data from a CSV file.
2. **Visualizes the price** – Plots the closing price over time to see the overall trend.
3. **Checks for patterns** – Uses ACF/PACF plots to study how prices are correlated with their past values.
4. **Makes the data stationary** – Stock prices usually trend up or down, which confuses the model. So the data is "differenced" (and log-transformed) to remove the trend.
5. **Tests for stationarity** – Runs the **Augmented Dickey-Fuller (ADF) test** to confirm the data is now stable enough to model.
6. **Finds the best model settings** – Uses `auto_arima` to automatically search for the best ARIMA parameters (p, d, q).
7. **Checks the model quality** – Looks at residual plots and runs a **Ljung-Box test** to make sure the model isn't missing any patterns.
8. **Splits the data** – Divides data into 80% training and 20% testing.
9. **Forecasts prices** – Trains ARIMA on the training data and predicts the test period.
10. **Evaluates accuracy** – Compares predictions vs. actual prices using **RMSE (Root Mean Squared Error)**, and checks in-sample vs out-of-sample performance.

## 🛠️ Requirements

Install these Python libraries before running the notebook:

```bash
pip install pandas numpy matplotlib seaborn statsmodels pmdarima scikit-learn
```

## ▶️ How to Run

1. Place your price data CSV file in the same folder (must have a `date` column and a `close` column).
2. Update the filename in the notebook if it's different from `NIFTY_50_5m_5Y.csv`.
3. Run the notebook cell by cell in Jupyter Notebook or Google Colab.

## 📊 Output

The notebook will show:
- Price trend charts
- Stationarity test results
- The best ARIMA model found automatically
- A forecast chart comparing predicted vs actual prices
- Accuracy scores (RMSE)

## ⚠️ Notes

- This project is for **learning and research purposes**, not real trading advice.
- ARIMA works best on data with clear, consistent patterns — real stock markets are noisy, so predictions won't always be accurate.
- You can swap in any other stock's price data as long as it has `date` and `close` columns.

## 📜 License

Feel free to use, modify, and learn from this project.
