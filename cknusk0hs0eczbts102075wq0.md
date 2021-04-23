## Time Series and Forecasting with Python code examples, Part II

We saw what a Time Series is and its main components in the previous post of this series on Time Series and Forecasting.

But we didn't talk anything about Forecasting!

Let's fix that. In this post, I'll introduce some very basic forecasting methods. They might seem too simple, but they are still used in practice. Furthermore, they are useful to build a baseline to compare more sophisticated methods with.

I strongly recommend this methodology of building simple baselines to evaluate further and more sophisticated models. Not just for Time Series Analysis but also in any branch of Machine Learning and Statistics.

 So, let's get started!

## The Naive Method

Think about the dumbest method to forecast future values. 

In just a minute you might discover the amazing Forecasting Naive Method! It just consists of predicting that the next value of the Series will be equal to the last recorded one.

For example, if our Series goes

$$X_1, X_1, ...., X_t$$

our next value will be 

$$X_{t+1} = X_t$$

Pretty naive isn't it?

Actually, it can be good enough when the Series is a Random Walk (when the data don't follow a pattern and is messy). So, don't underestimate it.

 I won't include any code in Python for this one because you can implement it in many ways and it is not necessary to use any third-party libraries.

## Naive Seasonal Method

When the data shows a strong seasonal pattern, we can slightly improve the previous naive method.

Let's suppose we have monthly data, and we want to predict what the value for the next month (let's call it February) will be. Then we'll assume that value will be equal to the value shown for past February (the same month of the last year). Assuming that our data shows a strong monthly seasonal pattern.

More formally, if we define the seasonal period ```p``` as the number of samples between one observation and the next one that lies in the same season, we can predict

$$X_{t + h} = X_k $$

where \\(k = t + h - p\\).

Assuming that every value before \\(X_{t + h}\\) is defined.

Examples of values for the seasonal period ```p``` are:

* 1, for *yearly* seasons
* 4, for *quarterly* seasons
* 12, for *monthly* seasons
* 52, for *weekly* seasons

And so on.

This is a simple Python code example of the Naive Seasonal Method:

```python
import numpy as np
import pandas as pd

def seasonal_naive_forecast(series, seasonal_period, forecast_horizon):

     if len(series) < seasonal_period:
         raise Error("There must be at least 'seasonal_period' observations")
     
     # We assume 'series' to be a pandas.Series object
     prev_season = series.iloc[-seasonal_period:]
     
     season_number = int(np.ceil(forecast_horizon/seasonal_period))
     
     next_seasons = np.tile(prev_season, season_number)
     
     # We were only asked to predict the next 'forecast_horizon' values
     return pd.Series(next_seasons[:forecast_horizon])
```

We improved the naive method to consider seasonality. Then, what about trending?

## Drift method

It would be great to allow the forecasting method to increase or decrease over time in cases when the data shows a trending behavior. That's what the drift method does.

The general formula would be:

$$
X_{t+h} = X_t + h*\frac{Xt - X1}{t - 1}
$$

Notice that the term $$\frac{Xt - X1}{t - 1}$$ is the slope of the line from the first observation to the last one. So, the drift method extrapolates that line into the future and assumes the data follows that trend.

The python code is omitted since it just consists of translating the previous formula to Python.

## Conclusions

In this post, we have explored the more basic forecasting methods. Although they are pretty simple, they might be good enough in many situations.

But remember that even when they were not a good solution, they are a great starting point to build our more sophisticated methods on top of.

In the upcoming posts of this series, we'll be talking about residual analysis, how to measure the performance of our models, and of course, how to build and use more robust and sophisticated models.

If you liked this post, consider subscribing to my newsletter so you don't miss an article from my blog. Do you prefer a simpler format and real-time interaction? Then, follow me on [Twitter](https://twitter.com/josejorgexl). I like to share more content there.

Stay tuned!

(Cover photo by <a href="https://unsplash.com/@javiestebaan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Javier Esteban</a> on <a href="https://unsplash.com/s/photos/time-series?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  )