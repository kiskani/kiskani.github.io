---
layout: post
title:  "Risk Measurement in Finance"
date:   2020-01-03 15:52:14 -0800
categories: finance
---
In this post, I will explain the methods of measuring risk in finance. One of the widely used concepts to measue the risk of an asset is the [Sharpe Ratio](https://en.wikipedia.org/wiki/Sharpe_ratio). I have implemented a code in Python to compute the Sharpe Ratio for some assets. 

{% highlight python %}
from pandas_datareader import data  # You will need to run "pip install pandas_datareader"
import holidays   # You will need to run "pip install holidays"
import datetime 
import numpy as np
{% endhighlight %}
{% highlight python %}
ONE_DAY = datetime.timedelta(days=1)
HOLIDAYS_US = holidays.US()

def previous_business_day(specific_date):
    previous_day = specific_date - ONE_DAY
    while previous_day.weekday() in holidays.WEEKEND or previous_day in HOLIDAYS_US:
        previous_day -= ONE_DAY
    return previous_day

def next_business_day(specific_date):
    next_day = specific_date + ONE_DAY
    while next_day.weekday() in holidays.WEEKEND or next_day in HOLIDAYS_US:
        next_day += ONE_DAY
    return next_day
{% endhighlight %}
{% highlight python %}
def get_historical_data(share, start_date='2019-01-01', end_date='2020-01-01'):
    panel_data = data.DataReader(share, 'yahoo', start_date, end_date)
    return panel_data
{% endhighlight %}
{% highlight python %}
def compute_average_annual_return(share, start_date='2019-01-02', end_date='2020-01-02'):
    panel_data = get_historical_data(share, start_date, end_date)
    adj_close = panel_data['Adj Close']
    
    start_date = datetime.datetime.strptime(start_date, '%Y-%m-%d')
    end_date = datetime.datetime.strptime(end_date, '%Y-%m-%d')
    
    while start_date.strftime('%Y-%m-%d') not in adj_close.index: 
        start_date = next_business_day(start_date)
    
    while end_date.strftime('%Y-%m-%d') not in adj_close.index: 
        end_date = previous_business_day(end_date)
    
    delta = end_date - start_date
    years = delta.days / 365
    
    start_price = adj_close.loc[start_date.strftime('%Y-%m-%d')]
    end_price = adj_close.loc[end_date.strftime('%Y-%m-%d')]
    
    return_value = (end_price/start_price)**(1/years) - 1
    return round(return_value, 4), start_date, end_date, delta, years
{% endhighlight %}
{% highlight python %}
def compute_daily_return_std_per_year(share, start_date='2019-01-02', end_date='2020-01-02'):
    panel_data = get_historical_data(share, start_date, end_date)
    last_day_price = None
    daily_returns = [] 
    for index, row in panel_data.iterrows():
        if last_day_price:
            daily_returns.append(row['Adj Close']/last_day_price - 1)
        last_day_price = row['Adj Close']
    return round(np.std(daily_returns)*np.sqrt(252), 4)
{% endhighlight %}
{% highlight python %}
def compute_sharpe(share, start_date='2019-01-02', end_date='2020-01-02'):
    avg, _, _, _, _ = compute_average_annual_return(share, start_date, end_date)
    std = compute_daily_return_std_per_year(share, start_date, end_date)
    return round(avg/std, 4)
{% endhighlight %}
{% highlight python %}
compute_sharpe('GOOG')
{% endhighlight %}
A summary of this code is available in [this notebook](https://github.com/kiskani/kiskani.github.io/blob/master/finance/2020/01/03/Finance-Notebook-1.ipynb).
