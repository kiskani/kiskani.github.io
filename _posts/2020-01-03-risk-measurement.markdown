---
layout: post
title:  "Risk Measurement in Finance"
date:   2020-01-03 15:52:14 -0800
categories: finance
---
A tribute to a great man who died on this day but whose legacy would never die. 

<img src="rose.jpeg" width="48">
![Tribute](rose.jpeg)

In this post, I will explain the methods of measuring risk in finance. One of the widely used concepts to measue the risk of an asset is the [Sharpe Ratio](https://en.wikipedia.org/wiki/Sharpe_ratio). I have implemented a code in Python to compute the Sharpe Ratio for some assets. 

{% highlight python %}
from pandas_datareader import data  # You will need to run "pip install pandas_datareader"
import holidays   # You will need to run "pip install holidays"
import datetime 
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
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
    try:
        panel_data = data.DataReader(share, 'yahoo', start_date, end_date)
    except: 
        panel_data = pd.DataFrame()
    return panel_data
{% endhighlight %}
{% highlight python %}
def compute_average_annual_return(share, start_date='2019-01-02', end_date='2019-12-31'):
    panel_data = get_historical_data(share, start_date, end_date)
    if panel_data.empty:
        return None, None, None, None, None
    
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
    if panel_data.empty:
        return None
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
    if not avg or not std:
        return
    return round(avg/std, 4)
{% endhighlight %}
{% highlight python %}
SP_500 = pd.read_excel('SP_500.xlsx', index_col=0)
SP_500_symbol_list = SP_500.index.to_list()[:-2]
iTickers = {}
iTickers["SPY"] = "SPDR S&P 500 ETF Trust"
iTickers["VOO"] = "Vanguard S&P 500 ETF"
iTickers["VTI"] = "Vanguard Total Stock Market ETF"
iTickers["BRK-B"] = "Berkshire Hathaway Inc."
iTickers["AAPL"] = "Apple Inc."
iTickers["BAC"] = "Bank of America"
iTickers["MSFT"] = "Microsoft"
iTickers["IVV"] = "iShares Core S&P 500 ETF"
{% endhighlight %}
{% highlight python %}
for symb in SP_500_symbol_list:  
    print("---------------\n", symb)
    sharpe_ratio = compute_sharpe(symb)
    print(SP_500.loc[symb]['Name'], sharpe_ratio)
{% endhighlight %}
{% highlight python %}
plt.figure(figsize=(14,4))
start_date='2000-01-31' 
for symb in iTickers:
    print(iTickers[symb])
    daa = get_historical_data(symb, start_date=start_date)
    sharpe_ratio = compute_sharpe(symb)
    #print(SP_500.loc[symb]['Name'], sharpe_ratio)
    print(iTickers[symb], sharpe_ratio)
    plt.plot(daa['Adj Close'])
    plt.grid(color='g', linestyle='-.', linewidth=0.5)
plt.legend([iTickers[sym] for sym in iTickers])
plt.savefig('well-known-tickers.png')
{% endhighlight %}
{% highlight text %}
SPDR S&P 500 ETF Trust
SPDR S&P 500 ETF Trust 2.5004
Vanguard S&P 500 ETF
Vanguard S&P 500 ETF 2.5269
Vanguard Total Stock Market ETF
Vanguard Total Stock Market ETF 2.4393
Berkshire Hathaway Inc.
Berkshire Hathaway Inc. 0.7833
Apple Inc.
Apple Inc. 3.42
Bank of America
Bank of America 1.9287
Microsoft
Microsoft 2.9596
iShares Core S&P 500 ETF
iShares Core S&P 500 ETF 2.5012
{% endhighlight %}
![Some SP 500 tickers](well-known-tickers.png)
{% highlight python %}
plt.figure(figsize=(14,4))
start_date='2010-01-02' 
end_date='2020-01-07' 
for symb in ['BRK-B','SPY']:
    daa = get_historical_data(symb, start_date=start_date, end_date=end_date)
    plt.plot(daa['Adj Close'])
    plt.grid(color='g', linestyle='-.', linewidth=0.5)
plt.legend(['BRK-B','SPY'])
plt.savefig('BRK_vs_SPY.png')
{% endhighlight %}
![Berkshire Hathaway vs SP 500](BRK_vs_SPY.png)
{% highlight python %}
def plot_stock_price(stock, start_date='2019-01-02', end_date='2019-12-31'):
    plt.figure(figsize=(14,4))
    daa = get_historical_data(stock, start_date=start_date, end_date=end_date)
    sharpe_ratio = compute_sharpe(stock)
    plt.plot(daa['Adj Close'])
    plt.grid(color='g', linestyle='-.', linewidth=0.5)
    plt.legend([stock])
    plt.savefig('{}_{}_{}.png'.format(stock, start_date, end_date))
{% endhighlight %}
{% highlight python %}
plot_stock_price('GOOG', start_date='2007-01-02', end_date='2010-01-02')
{% endhighlight %}
![Google](GOOG_2007-01-02_2010-01-02.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1900-01-01', end_date='1960-01-01')
{% endhighlight %}
![SP500_1900_1960](^GSPC_1900-01-01_1960-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1960-01-01', end_date='1970-01-01')
{% endhighlight %}
![SP500_1960_1970](^GSPC_1960-01-01_1970-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1970-01-01', end_date='1980-01-01')
{% endhighlight %}
![SP500_1970_1980](^GSPC_1970-01-01_1980-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1980-01-01', end_date='1990-01-01')
{% endhighlight %}
![SP500_1980_1990](^GSPC_1980-01-01_1990-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1990-01-01', end_date='2000-01-01')
{% endhighlight %}
![SP500_1990_2000](^GSPC_1990-01-01_2000-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='2000-01-01', end_date='2010-01-01')
{% endhighlight %}
![SP500_2000_2010](^GSPC_2000-01-01_2010-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='2010-01-01', end_date='2020-01-01')
{% endhighlight %}
![SP500_2010_2020](^GSPC_2010-01-01_2020-01-01.png)
{% highlight python %}
nasdaq = data.get_nasdaq_symbols()
print(nasdaq.shape)
nasdaq.head()
{% endhighlight %}
{% highlight python %}
def compute_stats(symb):
    ticker = data.get_quote_yahoo(symb) 
    ticker['c1_marketCap_bn_calc'] = ticker['price'] * ticker['sharesOutstanding'] / 1e9
    ticker['c2_marketCap_bn_orig'] = ticker['marketCap'] / 1e9
    ticker['c3_price_calc'] = ticker['epsForward'] * ticker['trailingPE']
    ticker['c4_earnings_bn_calc'] = ticker['epsForward'] * ticker['sharesOutstanding'] / 1e9
    return ticker
{% endhighlight %}
{% highlight python %}
compute_stats(['EXPE', 'GOOG', 'MSFT', 'AAPL', 'BB', 'JNPR', 'BRK-B', 'BRK-A'])
{% endhighlight %}
A summary of this code is available in [this notebook](https://github.com/kiskani/kiskani.github.io/blob/master/finance/2020/01/03/Finance-Notebook-1.ipynb).
