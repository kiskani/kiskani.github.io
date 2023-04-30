---
layout: post
title:  "Stock Valuation"
date:   2023-04-30 12:03:43 -0800
categories: finance
---
In this post, I will explain some methods of valuating stock prices. If we show the Free Cash Flow of this company at year $n$ from now by $FCF_n$ and if the discount rate is $r$, then the present value of the company will be equal to

$$
\textrm{PV} = \sum_{i=1}^{\infty} \frac{FCF_i}{(1+r)^i}
$$
However, it is not possible to estimate	cash flows forever so we need to estimate cash flows for a **growth	period** and	then estimate a	terminal value, to capture the value at the	end	of the period:

 First, let's see how we can calculate the terminal value of a **mature** company using it's growth rate $g$. 

$$C_y = P \frac{r(1+r)^T}{(1+r)^T-1} = 741750 \frac{0.02875 (1.02875^{30})}{1.02875^{30} -1} = \$37234.63 $$



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

