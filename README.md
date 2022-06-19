# Sales Forecating (Inter Hall Data Analytics 2021-22) 📈

## Problem Statement 🧠
The task is to improve forecasting using advanced time series and machine learning algorithms. Sales forecast for the month of June-2021 has to be made using the train data available from the time period of April-18 to May-21. There are a total of four features in the dataset, and they are as follows:
* **Warehouse ID:** This column contains numbers 1,2,3,4, which indicates
four different warehouses
* **Region:** This has the direction to which the warehouse supplies fans.
* **SKU ID:** Each id is unique and corresponds to different warehouses.
* **Apr-18 to May-21:** Each month in this period has the number of fans
sold.

## Scoring 🥇
The scoring is based on Mean Absolute Percentage Error (MAPE), which is defined as follows:
MAPE = ABS (Actual Sales - Forecasted Sales) / Actual Sales, if Actual Sales > 0
            0%, if Actual Sales and Forecasted Sales are both 0
            100%, if Actual Sales = 0 and Forecasted Sales < > 0)
     
## Final Model Approach 🎯
* The methods mentioned above were tried in our initial approach. The likes of Random Forest, XG boost regressor, ARIMA, SES, SARIMA, HOLT linear trend and FB prophet turned out to be much more **complex** when compared to our final approach.
* To arrive at our final model, we first **compared sales of consecutive months** and saw very strong correlations between them. This further led us to explore the heat map and we saw significant correlations upto the previous two months.
* We saw a very reduced MAPE when just predicting this month’s sales taking the previous month’s sales as reference when compared to the different complex models mentioned above.
* Now the question is should we predict this month’s sales (June’s data) as the most recent month's sales (May’s sales)? Average of last two months sales (( May+april)/2)? April’s sales? Or any other metric?
* We also had the knowledge of the fact that the **MAPE favours under-forecast** rather than over-forecast. This is caused by the fact that the percentage error can’t exceed 100% for forecasts that are too low, while there is no upper limit for forecasts which are too high.
* So to minimise our prediction we went with the following simplistic approach: **Predicted June’s sales = Minimum (April’s sales, May’s Sales)**
* This boosted our performance and we got an even less MAPE with this approach. For May 2021 we got **MAPE = 46.55 %**
