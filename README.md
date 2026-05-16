# Gold-Price-Prediction
Gold is a key financial asset and is widely regarded as a safe haven during periods of economic uncertainty, making it a preferred choice for investors seeking stability and portfolio diversification.


Define explanatory variables
An explanatory variable, also known as a feature or independent variable, is used to explain or predict changes in another variable. In this case, it helps predict the next-day price of the Gold ETF.

These are the inputs or predictors we use in a model to forecast the target outcome.

In this strategy, we start with two simple features: the 3-day moving average and the 9-day moving average of the Gold ETF. These moving average serve as smoothed representations of short-term and slightly longer-term trends, helping capture momentum or mean-reversion behavior in prices. Before using these features in modeling, we eliminate any missing values using the .dropna() function to ensure the dataset is clean and ready for analysis. The final feature matrix is stored in X.

However, this is just the beginning of the feature engineering process. You can extend X by incorporating additional variables that might improve the model's predictive power. These may include:

Technical indicators such as RSI (Relative Strength Index), MACD (Moving Average Convergence Divergence), Bollinger Bands, or ATR (Average True Range).
Cross-asset features, such as the price or returns of related ETFs like the Gold Miners ETF (GDX) or the Oil ETF (USO), which may influence gold prices through macroeconomic or sector-specific linkages.
Macroeconomic indicators such as inflation data (CPI), interest rates, and USD index movements can influence gold prices because gold is perceived as a safe-haven asset during times of economic uncertainty.
The process of identifying and constructing such variables is called feature engineering. Separately, selecting the most relevant variables for a model is known as feature selection.

The better your features reflect meaningful patterns in the data, the more accurate your forecasts are likely to be.

Define dependent variable
The dependent variable, also known as the target variable in machine learning, is the outcome we aim to predict. Its value is assumed to be influenced by the explanatory (or independent) variables. In the context of our strategy, the dependent variable is the price of the Gold ETF (GLD) on the following day.

In our dataset, the Close column contains the historical prices of the Gold ETF. This column serves as the target variable because we are building a model to learn patterns from historical features (such as moving averages) and use them to predict future GLD prices. We assign this target series to the variable y, which will be used during model training and evaluation.

To create the target variable, we apply the shift(-1) function to the Close column. This shifts the price data one step backward, making each row's target the next day's closing price. This approach enables the model to use today's features to forecast tomorrow's price.

Clearly defining the target variable is essential for any supervised learning problem, as it shapes the entire modelling objective. In this case, the goal is to forecast future movements in gold prices using relevant financial and economic signals.

Alternatively, instead of predicting the absolute price of gold, we can use gold returns as the target variable. Returns represent the percentage change in gold prices over a specified time period, such as daily, weekly, or monthly intervals.

Non-stationary variables in linear regression
In time series analysis, it's common to work with raw financial data such as stock or commodity prices. However, these price series are typically non-stationary, meaning their statistical properties like mean and variance change over time. This poses a significant challenge because many analytical techniques rely on the assumption that the data behaves consistently. When the data is non-stationary, its underlying structure shifts. Trends evolve, volatility varies, and historical patterns may not hold in the future.

Working with non-stationary data can lead to several problems:

Spurious Relationships: Variables may appear to be related simply because they share similar trends, not because there's a genuine connection.
Unstable Insights: Any patterns or relationships identified may not hold over time, as the data’s behaviour continues to evolve.
Misleading Forecasts: Predictive models built on non-stationary data often struggle to perform reliably in the future.
The core issue is that non-stationary processes do not follow fixed rules. Their dynamic nature makes it difficult to draw conclusions or make predictions that remain valid as conditions change. Before performing any serious analysis, it's crucial to test for stationarity and, if needed, transform the data to stabilize its behaviour.

Two Ways to Work with Non-Stationary Data

Rather than discarding non-stationary variables, there are two reliable strategies to handle them in linear regression models:

1. Make Variables Stationary (Differencing Approach)

One common method is to transform the data to make it stationary. This is often done by focusing on changes in values. For example, price series can be converted into returns or differences. This transformation helps stabilize the mean and reduces trends or seasonality. Once the data is transformed, it becomes more suitable for linear modeling because its statistical properties remain consistent over time.

2. Use Original Non-Stationary Series (Cointegration Approach)

The second strategy allows us to use the original non-stationary series without transformation, provided certain conditions are met. Specifically, it involves checking whether the variables, when combined in a particular way, share a long-term equilibrium relationship. This concept is known as cointegration.

Even if the individual variables are non-stationary, their linear combination might be stationary. If this is the case, the residuals from the regression (the differences between actual and predicted values) remain stable over time. This stability makes the regression valid and meaningful, as it reflects a genuine relationship rather than a statistical coincidence.

In our analysis, we will use this second method by testing for residual stationarity to confirm that the regression setup is appropriate.

The time series S_3 (3-day moving average) and next_day_price, as well as S_9 (9-day moving average) and next_day_price, are cointegrated. Thus, we can proceed with running a linear regression directly without transforming the series to achieve stationarity.

Why You Can Run the Regression Directly?

Cointegration implies that there is a stable, long-term relationship between the two non-stationary series. This means that while the individual series may each contain unit roots (i.e., be non-stationary), their linear combination is stationary and running an Ordinary Least Squares (OLS) regression will not lead to a spurious regression. This is because the residuals of the regression (i.e., the difference between the predicted and actual values) will be stationary.

Key Points to Remember

As cointegration already ensures a valid statistical relationship, making OLS appropriate for estimating the parameters, there is no need to difference the series to make them stationary before running the regression

The regression run between S_3 (or S_9) and next_day_price will capture a valid long-term equilibrium relationship, which cointegration confirms.

Split the data into train and test dataset
In this step, we split the predictors and output data into train and test data. The training data is used to create the linear regression model, by pairing the input with expected output.

Model training is performed on the training dataset, where the model learns from the features and labels.

The test data is used to estimate how well the model has been trained. Comparing different models and evaluating their training time and accuracy is an important part of the model selection process. Model evaluation, including the use of validation sets and cross-validation, ensures the model generalizes well to unseen data.







