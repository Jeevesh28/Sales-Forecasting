# Sales Forecasting: When the Simplest Model Wins

Monthly sales forecasting for electric fans across four warehouses. After trying ARIMA, SARIMA, Prophet, Holt's linear trend and tree ensembles, the best submission was a one-line rule built from two observations about the data and the scoring metric.

> **Inter Hall Data Analytics 2021-22**, IIT Kharagpur · **1st runner-up**
> <!-- Team: add teammate names here -->

**Stack:** Python · pandas · statsmodels · Prophet · scikit-learn · XGBoost

---

## Problem

Forecast June 2021 sales for every SKU, using monthly sales from April 2018 to May 2021.

| Field | Meaning |
|---|---|
| Warehouse ID | One of four warehouses (1 to 4) |
| Region | Direction the warehouse supplies |
| SKU ID | Product identifier, unique per warehouse |
| Apr-18 ... May-21 | Units sold in each month |

**Scoring:** Mean Absolute Percentage Error (MAPE), per row:

```
|actual - forecast| / actual    if actual > 0
0%                              if actual = 0 and forecast = 0
100%                            if actual = 0 and forecast != 0
```

## Approach

**1. Benchmark the standard models.** Random Forest, XGBoost regression, ARIMA, SARIMA, simple exponential smoothing, Holt's linear trend and Prophet were all tried first. None beat a naive baseline by enough to justify its complexity.

**2. Look at the structure of the data.** Comparing consecutive months showed very strong correlation between a month's sales and the previous month's, and the correlation heatmap showed it held up to two months back. Simply predicting last month's sales already beat the complex models on MAPE.

**3. Exploit the asymmetry in the metric.** MAPE penalizes over-forecasting more than under-forecasting: an under-forecast's error is capped at 100%, while an over-forecast's error is unbounded. So when uncertain, it pays to forecast low.

**4. Final rule:**

```
forecast(June) = min(sales(April), sales(May))
```

It uses the two most correlated months, and taking the minimum leans into MAPE's preference for under-forecasting.

## Results

Validated by forecasting May 2021 from earlier months: **MAPE = 46.55%**, lower than every model tried in step 1.

The takeaway is about method rather than the rule itself: understanding the scoring function and the autocorrelation structure beat a sweep over model families.

## Repository layout

```
Inter Hall Data Analytics 2021-22.ipynb   analysis, model comparison, final rule
interhall_train_data.xlsx                 training data
interhall_submission.csv                  final June 2021 forecast
Report.pdf · Presentation.pdf             competition write-up and slides
Inter Hall Problem Statement.pdf · Rules.pdf
```

## Limitations

- The rule has no trend or seasonality term, so it would lag a sustained ramp up or down.
- It is tuned to MAPE specifically; under a symmetric metric like RMSE, the `min` bias would hurt.
