---
layout: post
title:  "Gradient Bandits"
date:   2019-12-30 19:13:11 -0800
categories: multi-armed-bandits
---
In this post, I will explain the fundamentals of gradient bandits. First, I will explain the `Log-likelihood trick` (or `REINFORCE` trick, Williams 1992). Assume that $\theta$ is the parameter
vector and $R_t$ is the `reward` at time t which is a function of $\theta$. Also, let $A_t$ be the `action` at time t, $\pi_{\theta}(a)$ be the `policy` which is parametrized on $\theta$ and we want to find it directly through gradient updates. Letting

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

In the following, we will use this trick to derive the gradient update equation for the case of softmax policy preferences (derivation from [Richard Sutton](https://www.amazon.com/Reinforcement-Learning-Introduction-Adaptive-Computation/dp/0262039249)). If $H_t(a)$ shows the action `preference` in a softmax setting we have,

$$
\pi(a) = \frac{\exp(H_t(a))}{\sum_{b} \exp(H_t(b))}.
\label{eq_def_pi}
$$

Gradient ascent updates the preference as 

$$
H_{t+1}(a) \leftarrow H_t(a) + \alpha \frac{\partial \mathbb{E}[R_t]}{\partial H_t(a)}
$$

where the expected reward is computed as 

$$
\mathbb{E}[R_t] = \sum_{x} \pi_t(x) q_{\star}(x)
$$

hence, 

$$
\frac{\partial \mathbb{E}[R_t]}{\partial H_t(a)} = \frac{\partial}{\partial H_t(a)} \left[ \sum_x \pi_t(x) q_{\star}(x) \right] = \sum_x q_{\star}(x) \frac{\partial \pi_t(x)}{\partial H_t(a)} = \sum_x (q_{\star}(x) - B_t) \frac{\partial \pi_t(x)}{\partial H_t(a)}
$$

Where $B_t$ called the `baseline`, can be any scalar that does not depend on $x$. The baseline can be included without changing the equality because the gradient sums
to zero over all the actions, $\sum_x \frac{\partial \pi_t(x)}{\partial H_t(a)} = 0$. As $H_t(a)$ is changed, some actions 
probabilities go up and some go down, but the sum of the changes must be zero because the sum of the probabilities is always one. If we multiply each term of the sum by $\frac{\pi_t(x)}{\pi_t(x)}$, we have 

$$
\frac{\partial \mathbb{E}[R_t]}{\partial H_t(a)} = \sum_x \pi_t(x) (q_{\star}(x) - B_t) \frac{\partial \pi_t(x)}{\partial H_t(a)} \frac{1}{\pi_t(x)}
$$

The equation is now in the form of an expectation, summing over all possible values $x$ of the random variable $A_t$, then multiplying by the probability of taking those values, we have 

$$
\frac{\partial \mathbb{E}[R_t]}{\partial H_t(a)} = \mathbb{E} \left[ \left(q_{\star}(A_t) - B_t \right) \frac{\partial \pi_t(A_t)}{\partial H_t(a)} \frac{1}{\pi_t(A_t)} \right]
$$

Noting that 
$q_{\star}(A_t) = \mathbb{E}[R_t | A_t]$ 
and choosing 
$B_t = \bar{R}_t$ 
where 
$\bar{R}_t$
is the average of all the rewards up through and including time $t$, we have 

$$
\frac{\partial \mathbb{E}[R_t]}{\partial H_t(a)} = \mathbb{E} \left[ \left(R_t - \bar{R}_t \right) \frac{\partial \pi_t(A_t)}{\partial H_t(a)} \frac{1}{\pi_t(A_t)} \right]
$$ 

Using equation \eqref{eq_def_pi} we can prove that 

$$
\frac{\partial \pi_t(A_t)}{\partial H_t(a)} = \pi_t(A_t) \left( \mathbb{1}_{a==A_t}- \pi_t(a)\right) 
$$

Hence, 

$$
\frac{\partial \mathbb{E}[R_t]}{\partial H_t(a)} = \mathbb{E} \left[ \left(R_t - \bar{R}_t \right) \left( \mathbb{1}_{a==A_t}- \pi_t(a)\right) \right]
$$

Now that we wrote this in the form of a nice expectation, we can sample in each step and therefore, we can write the update equation for stochastic gradient ascent algorithm as 

$$
H_{t+1}(a) \leftarrow H_t(a) + \alpha \left( R_t - \bar{R}_t \right)\left( \mathbb{1}_{a==A_t}- \pi_t(a)\right) \qquad \forall a
$$

This equation can be written as 

$$
\begin{align}
H_{t+1}(A_t) &\leftarrow H_t(A_t) + \alpha (R_t - \bar{R}_t) \left( 1 - \pi_t(A_t) \right) \nonumber \\
H_{t+1}(a) &\leftarrow H_t(a) - \alpha (R_t - \bar{R}_t) \pi_t(a) \qquad \textrm{if} ~ a \neq A_t
\end{align} 
$$

This means that we select a certain action and we update all the preferences. The algorithm updates all action preferences in each step. The $\bar{R}_t$ term serves as a baseline with which the reward is compared. If the reward is higher than the baseline, then the probability of taking $A_t$ in the future is increased, and if the reward is below baseline, then probability is decreased. The non-selected actions move in the opposite direction. The baseline helps reduce the varaince.

Here is a simple Python implementation of this algorithm (inspired by [this post.](https://gist.github.com/khanrc/fe36cd1e7e60f61c90b5a6d484fadb7a)). 

{% highlight python %}
import numpy as np
import math
import matplotlib.pyplot as plt
{% endhighlight %}
{% highlight python %}
def get_rewards():
    mean = np.array([-2, 1, 0, -3]) 
    return np.random.randn(4) + mean
{% endhighlight %}
{% highlight python %}
def softmax(H):
    h = H - np.max(H)
    exp = np.exp(h)
    return exp / np.sum(exp)
{% endhighlight %}
{% highlight python %}
def gradient_bandit(N):
    H = np.zeros(4)  
    r_hist = []
    alpha = 0.1  
    for t in range(1, N):
        policy = softmax(H) # policy pi
        # sampling (choice) action by policy
        a = np.random.choice(4, p=policy) 
        rewards = get_rewards()
        r = rewards[a] # R_t (reward for chosen action)
        r_hist.append(r)
        avg_r = np.average(r_hist)
        # update a == A_t (chosen action)
        H[a] = H[a] + alpha*(r-avg_r)*(1-policy[a])
        # update a != A_t (non-chosen action)
        H[:a] = H[:a] - alpha*(r-avg_r)*policy[:a]
        H[a+1:] = H[a+1:] - alpha*(r-avg_r)*policy[a+1:]
    
    return softmax(H), r_hist
{% endhighlight %}
{% highlight python %}
opt_policy, r_hist = gradient_bandit(100)
plt.plot(opt_policy, 'o')
{% endhighlight %}
![Gradient Bandit](gradient-bandit.png)
A summary of this code is available in [this notebook.](https://github.com/kiskani/kiskani.github.io/blob/master/multi-armed-bandits/2019/12/29/Thompson-sampling.ipynb)
