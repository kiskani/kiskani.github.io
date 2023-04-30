---
layout: post
title:  "Stock Valuation"
date:   2023-04-30 12:03:43 -0800
categories: finance
---
In this post, I will explain some methods of valuating stock prices. If we show the Free Cash Flow of this company at year $n$ from now by $\textrm{FCF}_n$ and if the discount rate is $r$, then the **Present Value of Future Free Cash Flows (PVFFCF)** of the company will be equal to

$$
\textrm{PVFFCF} = \sum_{i=1}^{\infty} \frac{\textrm{FCF}_i}{(1+r)^i}
$$

However, it is not possible to estimate	cash flows forever so we need to estimate cash flows for a **growth	period** and then estimate a **Terminal Value of Future Free Cash Flows (TVFFCF)**, to capture the value at the end of the period. Assume that the company enters the **maturity period** at the end of year $N$, then the Present Value of Future the company can be estimated as 

$$
\textrm{PVFFCF} = \sum_{i=1}^{N} \frac{\textrm{FCF}_i}{(1+r)^i} + \frac{\textrm{TVFFCF}}{(1+r)^{N}}
$$

One way to estimate the terminal value is to use the *stable growth model* or *[perpetuity growth model](https://en.wikipedia.org/wiki/Terminal_value_(finance))* or *[Gordon growth model](https://en.wikipedia.org/wiki/Dividend_discount_model)* or *[Dividend Discount Model (DDM)](https://en.wikipedia.org/wiki/Dividend_discount_model)*. In this model, we assume that when the comany enters its mature phase of operations, its free cash flow grows at a constant rate of $g$. We assumed that the company just entered the maturity phase at the end of year $N$, so we can say that the company's free cash flow at year $N+i$ for $i=1,2,...$ will be equal to 

$$\textrm{FCF}_{N+i} = (1+g)^i \textrm{FCF}_N$$

Since want to calculate the terminal value at the beginning of the maturity phase, we need to discount $\textrm{FCF}_{N+i}$ to the end of year $N$. Therefore, we have

$$
\textrm{TVFFCF} = \sum_{i=1}^{\infty} \frac{\textrm{FCF}_{N+i}}{(1+r)^i} = \sum_{i=1}^{\infty}   \frac{(1+g)^i \textrm{FCF}_N}{(1+r)^i} =\textrm{FCF}_N \sum_{i=1}^{\infty} \left( \frac{1+g}{1+r}\right)^i = \left( \frac{1+g}{r-g} \right) \textrm{FCF}_N 
$$

Notice that the assumption is that the overall economy is composed of high growth and stable growth firms and therefore the growth rate of the latter is less than the average growth rate of the economy. Therefore, in the above formula the stable growth rate $g$ should be set smaller than the discount rate $r$. However, $g$ can be negative implying that the firm is losing value over time. 

Finding $\textrm{TVFFCF}$ enables us to find $\textrm{PVFFCF}$. We can then subtract the company's **Total Debt (DEBT)** from $\textrm{PVFFCF}$ and add the sum of **Cash and Cash Equivalents (CASH)** to it to find the **Equity Value (EQUITY)** of the company.

$$
\textrm{EQUITY} = \textrm{PVFFCF} - \textrm{DEBT} + \textrm{CASH}
$$

The stock price of the company is then found by dividing the equity value by the **Number of Shares Outstanding (SHARES)**

$$
\textrm{Stock Price} = \frac{\textrm{EQUITY}}{\textrm{SHARES}}.
$$

{% highlight python %}
plt.figure(figsize=(20,10))
plt.plot(years, yearly_interest, '-k', label = "Yearly Interest Paid")
plt.xlabel('Year passed.')
plt.ylabel('Dollar amount')
plt.title('Yearly Interest plot')
plt.grid(True)
plt.legend(loc = "upper right")
plt.show()
{% endhighlight %}

![yearly-interest](yearly-interest.png)

