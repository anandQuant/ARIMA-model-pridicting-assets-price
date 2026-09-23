# 📈 Stock Price Forecasting with ARIMA

This project uses a statistics model called **ARIMA** to look at a stock's past prices and try to guess (forecast) what the price might do next. It also checks how good the model actually is, using real test data.

Everything runs inside a Jupyter Notebook (`ARIMA_app.ipynb`).

---

## 🧠 What is ARIMA? (In Plain Words)

ARIMA stands for **AutoRegressive Integrated Moving Average**. Don't worry about the fancy name — think of it like this:

- It looks at how a stock price moved in the past.
- It looks at patterns in the ups and downs.
- It uses those patterns to make a smart guess about future prices.

ARIMA has 3 settings, written as **(p, d, q)**:
- **p** — how much the model looks at *past values*
- **d** — how many times we had to "difference" the data to make it stable (see Step 2)
- **q** — how much the model looks at *past errors/noise*

You don't need to pick these yourself — the notebook finds the best combination automatically.

---

## ⚙️ Requirements

Install these Python packages before running the notebook:

```bash
pip install pmdarima yfinance pandas numpy matplotlib statsmodels scikit-learn
```

The first cell in the notebook also runs `!pip install pmdarima` for you.

---

## 🚀 How to Use This Notebook (Step by Step)

### Step 0 — Setup
Run the first two cells to install and import all the required libraries.

### Step 1 — Load the Data
At the top of Step 1, you can choose where your stock data comes from:

```python
DATA_SOURCE = "yfinance"   # or "csv"
TICKER = "GOOGL"           # stock symbol, e.g. AAPL, MSFT, TSLA
START_DATE = "2020-01-01"
END_DATE = "2023-12-31"
```

- **"yfinance"** → automatically downloads real stock prices from Yahoo Finance for the ticker/dates you choose.
- **"csv"** → uses your own local file instead (set `CSV_FILE_PATH` to point to it). Your CSV needs a `Date` column and a price column (like `Close`).

Just edit these settings, then run the cell. You'll see a chart of the raw stock price.

### Step 2 — Check if the Data is "Stationary"
Stock prices normally trend up or down over time — that's called **non-stationary**, and ARIMA doesn't like that. This step:

1. Runs a statistical test (called the **ADF test**) on the raw price.
2. Converts the price into **daily log returns** (basically: "how much did the price change, in percentage terms, from one day to the next?"). This usually removes the trend.
3. Runs the ADF test again on the returns to confirm they're now stable.

**How to read the ADF test result:**
- **p-value ≤ 0.05** → the data IS stable/stationary. Good, ready for modeling.
- **p-value > 0.05** → the data is NOT stable yet. It still has a trend.

You'll normally see the raw price fail this test, and the log returns pass it — that's expected and healthy.

### Step 3 — Split Into Training and Testing Data
The notebook uses:
- **80% of the data** ("training set") to teach the model.
- **20% of the data** ("testing set") to check if the model's guesses actually match reality, since we already know what really happened in that period.

Think of training data as "practice questions" and testing data as the "exam" the model hasn't seen the answers to.

### Step 4 — Let the Computer Pick the Best Model Settings
This step uses `auto_arima`, which automatically tries lots of combinations of (p, d, q) and picks the one that fits the training data best (using something called AIC — lower is better, you don't need to calculate this yourself).

At the end, it prints something like:

```
Best Selected Model Order: ARIMA(2, 1, 2)
```

That just means it found the (p, d, q) combo that works best for this stock's data.

### Step 5 — Check the Model's "Report Card" (Diagnostics)
Once the model is trained, we need to make sure it's not missing any obvious patterns. This step shows 4 diagnostic charts and runs one more statistical test (the **Ljung-Box test**) on the leftover errors ("residuals").

**How to read this:**
- If the leftover errors look like random noise (no visible pattern), the model did a good job.
- **Ljung-Box p-value > 0.05** → ✅ Errors are random noise. Model is trustworthy.
- **Ljung-Box p-value ≤ 0.05** → ⚠️ There's still a pattern the model missed. Forecasts might not be fully reliable.

The notebook prints a plain message for you either way:
- `"Diagnostics Passed: Errors are random White Noise"` = good sign
- `"Diagnostics Warning: Autocorrelation present in errors"` = model may need improvement

### Step 6 — Forecast the Future & Check Accuracy
Finally, the model predicts prices for the "test" period (the 20% it never saw), and compares those guesses to what actually happened.

You'll see a chart with:
- **Blue line** = recent real training prices (for context)
- **Black line** = actual real prices (the "correct answers")
- **Red dashed line** = the model's forecast
- **Pink shaded area** = the 95% confidence range — the model is saying "I'm 95% confident the real price will land somewhere in this pink zone."

**How to read the accuracy numbers at the end:**
- **RMSE (Root Mean Squared Error)** — on average, how far off the forecast was from the real price, in dollars. Bigger mistakes are punished more heavily by this metric.
- **MAE (Mean Absolute Error)** — the average size of the forecast mistake, in dollars, treating all mistakes equally.

**Simple rule of thumb:** Lower RMSE and MAE = better forecast. If these numbers are small compared to the actual stock price, the model did reasonably well. If they're large, take the forecast with a grain of salt.

---

## ⚠️ Important Notes / Limitations

- **This is not financial advice.** Stock markets are affected by news, emotions, and events that a math model can't predict.
- ARIMA is best at short-term forecasts, not long-term investment decisions.
- A model that "passes" the diagnostic tests is *statistically sound*, but that doesn't guarantee future prices will actually follow the forecast — no model can promise that with real markets.

---

## 📁 Files

- `ARIMA_app.ipynb` — the full notebook with all 6 steps described above.
- `google_stock.csv` (optional) — only needed if you set `DATA_SOURCE = "csv"`.

---

## 🙋 Quick Troubleshooting

| Problem | Likely Fix |
|---|---|
| `yfinance` download fails or returns empty | Check your internet connection or try a different date range/ticker |
| CSV mode gives a column error | Make sure your CSV has a `Date` column and one price column (e.g. `Close`) |
| `pmdarima` won't install | Try `pip install pmdarima --upgrade` or use a virtual environment |
| Forecast looks way off | That's normal for volatile stocks — try a longer training period or a different ticker |
