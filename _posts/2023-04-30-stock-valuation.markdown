---
layout: post
title:  "Stock Valuation"
date:   2023-04-30 12:03:43 -0800
categories: finance
---
In this post, I will explain some methods of valuating stock prices. If we show the Free Cash Flow of this company at year $n$ from now by $\textrm{FCF}_n$ and if the discount rate is $r$, then the **Intrinsic Value (IV)** of the company will be equal to

$$
\textrm{IV} = \sum_{i=1}^{\infty} \frac{\textrm{FCF}_i}{(1+r)^i}
$$

However, it is not possible to estimate	cash flows forever so we need to estimate cash flows for a **growth	period** and	then estimate a	**Terminal Value (TV)**, to capture the value at the	end	of the period. When we find the terminal value, then the intrinsic value of the company can be estimated as 

$$
\textrm{IV} = \sum_{i=1}^{N} \frac{\textrm{FCF}_i}{(1+r)^i} + \frac{\textrm{TV}}{(1+r)^N}
$$

Now, the interesting question is how to estimate the terminal value. A sound approach to estimate this value is to use the *stable growth model*. In this model, we assume that when the comany enters its **mature** phase of operations, its cash flow grows at a constant rate of $g$. 




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

