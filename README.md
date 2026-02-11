# Gold Price Forecasting with LSTM

### Price vs Log Return vs 10-Day Log Return

## 📌 Project Overview

This project investigates how target formulation affects deep learning performance in financial time series forecasting.

Using historical gold price data, an LSTM model is trained under three different target definitions:

1. **Adjusted Close Price**
2. **1-Day Log Return**
3. **10-Day Log Return**

The goal is to analyze how non-stationarity, scale changes, and prediction horizon influence model behavior and generalization.

---

## ⚠️ Problem Observed: Predicting Raw Prices

When predicting raw Adjusted Close prices:

* Training performance appeared strong.
* Test performance collapsed.
* The model failed to generalize to higher price regimes.

This happened because gold prices in the test period reached levels never seen in training data. Neural networks struggle to extrapolate beyond learned scale ranges, especially in non-stationary financial series.

This demonstrates how raw price modeling can lead to misleading training performance.

---

## 🔁 Log Return Transformation

Log return is defined as:

[
r_t = \log\left(\frac{P_t}{P_{t-1}}\right)
]

Log returns:

* Remove price-level scale
* Improve stationarity
* Stabilize variance
* Prevent regime-based prediction collapse

However, daily log returns are highly noisy. When trained with MSE, the model converges toward predicting the conditional mean (close to zero), resulting in flat predictions.

---

## 📈 10-Day Log Return

To reduce noise and improve learnability, the target was reformulated as:

[
r_{10d} = \log\left(\frac{P_{t+10}}{P_t}\right)
]

This measures the total compounded return over the next 10 days.

Benefits:

* Aggregates short-term noise
* Amplifies medium-term trend signal
* Improves signal-to-noise ratio
* Enables more meaningful structure learning

With this formulation, the LSTM successfully captured broader trend movements compared to single-day forecasting.

---

## 🧠 Key Takeaways

* Raw price prediction is highly sensitive to non-stationarity.
* Log returns are more statistically stable for modeling.
* Financial returns are noisy and difficult to predict at short horizons.
* Increasing forecast horizon improves learnability.
* Target engineering can matter more than model complexity.
