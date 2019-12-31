---
layout: post
title:  "Gradient Bandits"
date:   2019-12-30 19:13:11 -0800
categories: multi-armed-bandits
---
In this post, I will explain the fundamentals of gradient bandits. First, I will explain the `Log-likelihood trick` (or `REINFORCE` trick, Williams 1992). Assume that $\theta$ is the parameter
vector and $R_t$ is the `reward` at time t which is a function of $\theta$. Also, let $A_t$ be the `action` at time t, $\pi_{\theta}(a)$ be the `policy` which is parametrized on $\theta$ and we want to find it directly through gradient updates. If $H_t(a)$ shows the action `preference` in a softmax setting we have,

$$
\pi(a) = \frac{\exp(H_t(a))}{\sum_{b} \exp(H_t(b))}.
$$

Letting

$$
q(a) \triangleq \mathbb{E}[R_t | A_t = a].
$$

and running gradient ascent on parameters $\theta$ to maximize the expected reward, we have 

$$
\theta \leftarrow \theta + \alpha \nabla_{\theta} \mathbb{E}[R_t | \theta].
$$ 

However, this is not quite possible since we need to first take the average and then take the gradient. We cannot simply do the sampling. Hence, we can write the second term as 

$$
\begin{align}
\nabla_{\theta} \mathbb{E}[R_t | \theta] &= \nabla_{\theta} \sum_{a} \pi_{\theta}(a) \mathbb{E}[R_t | A_t = a] \nonumber \\
&= \sum_{a} q(a) \nabla_{\theta} \pi_{\theta}(a) \nonumber \\
&=  \sum_{a} q(a) \frac{\pi_{\theta}(a)}{\pi_{\theta}(a)} \nabla_{\theta} \pi_{\theta}(a) \nonumber \\
&= \sum_{a}\pi_{\theta}(a) q(a) \frac{\nabla_{\theta}\pi_{\theta}(a)}{\pi_{\theta}(a)}  \nonumber \\
&= \mathbb{E} \left[ R_t \frac{\nabla_{\theta} \pi_{\theta}(A_t)}{\pi_{\theta}(A_t)} \right] \nonumber \\
&= \mathbb{E} \left[ R_t \nabla_{\theta} \log \pi_{\theta}(A_t) \right].
\end{align}
$$ 

This allows us to easily perform stochastic gradient descent since now we are sampling from the values of the gradient. In this case, the stochastic gradient update formula looks like 

$$
\theta \leftarrow \theta + \alpha R_t \nabla_{\theta} \log \pi_{\theta}(A_t). 
$$

The policy can be a very complex function and it can be a deep network as we know how to compute gradients on deep networks. This is what makes this approach unique. We only need to sample the 
reward and its corresponding action for each iteration of the gradient ascent algorithm.  

{% highlight python %}
import numpy as np
import math 
import matplotlib.pyplot as plt
{% endhighlight %}
%![Thompson Sampling](thompson.png)
%A summary of this code is available in [this notebook.](https://github.com/kiskani/kiskani.github.io/blob/master/multi-armed-bandits/2019/12/29/Thompson-sampling.ipynb)
