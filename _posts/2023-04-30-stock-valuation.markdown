---
layout: post
title:  "Stock Valuation"
date:   2023-04-30 12:03:43 -0800
categories: finance
---
In this post, I will explain some methods of valuating stock prices. First, let's see how we can calculate the terminal value of a company based on its 

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

