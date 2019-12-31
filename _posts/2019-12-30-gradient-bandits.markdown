---
layout: post
title:  "Gradient Bandits"
date:   2019-12-30 19:13:11 -0800
categories: multi-armed-bandits
---
In this post, I will explain the fundamentals of gradient bandits. First, I will explain the `Log-likelihood trick` (or `REINFORCE` trick, Williams 1992). Assume that $\theta$ is the parameter
vector and $R_t$ is the `reward` at time `t` which is a function of $\theta$. Also, let $A_t$ be the `action` at time `t`. Let,
$
q(a) \triangleq \mathbb{E}[R_t | A_t = a]
$


$$ E = M\cdot c^2 \label{eq:mc2} $$

{% highlight python %}
import numpy as np
import math 
import matplotlib.pyplot as plt
{% endhighlight %}
This is the implementation of the greedy algorithm in the paper
{% highlight python %}
def greedy_update_posterior_params(T, rewards):
    beta = [1 for _ in rewards]
    alpha = [1 for _ in rewards]
    bins = np.linspace(0, 1, 100)
    for t in range(1,T+1):
        theta = []
        for k in range(len(rewards)):
            theta.append(alpha[k]/(alpha[k]+beta[k]))
        max_idx = max(range(len(rewards)), key=lambda i:theta[i])
        r = rewards[max_idx]
        beta[max_idx] += 1-r
        alpha[max_idx] += r
        if t % 100 == 0:
            plt.figure(figsize=(14,4))
            for j in range(len(rewards)):
                armj = [np.random.beta(alpha[j], beta[j]) for _ in range(1000)]
                plt.hist(armj, bins, alpha=0.5, label='arm {}'.format(j))
            plt.legend(loc='upper right')
            plt.show()
         
    return alpha, beta
{% endhighlight %}
Here is also the implementation of the Thompson Sampling algorithm for the Bernoulli bandit with Beta prior distribtuion. 
{% highlight python %}
def thompson_sampling_update_posterior_params(T, rewards):
    beta = [1 for _ in rewards]
    alpha = [1 for _ in rewards]
    bins = np.linspace(0, 1, 100)
    for t in range(1,T+1):
        theta = np.random.beta(alpha, beta).tolist()
        max_idx = max(range(len(rewards)), key=lambda i:theta[i])
        r = rewards[max_idx]
        beta[max_idx] += 1-r
        alpha[max_idx] += r
        if t % 100 == 0:
            plt.figure(figsize=(14,4))
            for j in range(len(rewards)):
                armj = [np.random.beta(alpha[j], beta[j]) for _ in range(1000)]
                plt.hist(armj, bins, alpha=0.5, label='arm {}'.format(j))
            plt.legend(loc='upper right')
            plt.show()
            
    return alpha, beta
{% endhighlight %}
Here is the result of running these algorithms
{% highlight python %}
alpha, beta = greedy_update_posterior_params(500,  [0.1,0.8,0.6,0.3])
{% endhighlight %}
![Greedy Algorithms](greedy.png)
{% highlight python %}
alpha, beta = thompson_sampling_update_posterior_params(500,  [0.1,0.8,0.6,0.3])
{% endhighlight %}
![Thompson Sampling](thompson.png)
A summary of this code is available in [this notebook.](https://github.com/kiskani/kiskani.github.io/blob/master/multi-armed-bandits/2019/12/29/Thompson-sampling.ipynb)
