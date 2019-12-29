---
layout: post
title:  "Multi-Armed Bandits"
date:   2019-12-28 20:39:11 -0800
categories: Reiforcement Learning
---
In this post, I will try to explain some interesting ideas around multi-armed bandits. Defining a simple multi-armed-bandit as follows
{% highlight python %}
def multiArmedBandit(k):
    if k == 0:
        return 2*np.random.randn()+1
    elif k == 1:
        return np.random.randn()+2
    elif k == 2:
        return 3*np.random.randn()
    elif k == 3:
        return np.random.uniform(-5,5)
    else:
        return None 
{% endhighlight %}
It is clear that arm 1 has the largest expected value of reward which is equal to 2. Hence, the optimal value `v*` is equal to 2. This value will be used to compute the regret. Also we have 
* q(0) = 1    =>  regret = 1 
* q(1) = 2    =>  regret = 0 
* q(2) = 0    =>  regret = 2 
* q(3) = 0    =>  regret = 2 

The following piece of code can be used to plot the distribution of different arms
{% highlight python %}
N = 100000 
arm0 = [multiArmedBandit(0) for _ in range(N)]
arm1 = [multiArmedBandit(1) for _ in range(N)]
arm2 = [multiArmedBandit(2) for _ in range(N)]
arm3 = [multiArmedBandit(3) for _ in range(N)]
bins = np.linspace(-10, 10, N/500)
plt.figure(figsize=(14,4))
plt.hist(arm0, bins, alpha=0.5, label='arm 0')
plt.hist(arm1, bins, alpha=0.5, label='arm 1')
plt.hist(arm2, bins, alpha=0.5, label='arm 2')
plt.hist(arm3, bins, alpha=0.5, label='arm 3')
plt.legend(loc='upper right')
plt.show()
{% endhighlight %}
This is a plot of the distribution of different arms
#![Arm distributions](arm_distributions.png)

