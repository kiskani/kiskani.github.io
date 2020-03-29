---
layout: post
title:  "Estimation theory basics"
date:   2020-03-29 11:17:44 -0800
categories: estimation
---

In this post, I will explain some of the basic and fundamental theorems and concepts in estimation theory. Mostly, I am putting the summary of the great book of [Steven Kay](https://www.amazon.com/Fundamentals-Statistical-Signal-Processing-Estimation/dp/0133457117/) in here.

## Minimum Variance Unbiased Estimation

Assume that we want to estimate a parameter $\theta$ in a model. An estimation $\hat{\theta}$ is said to be <em>unbiased</em> if 

$$
\mathbb{E}[\hat{\theta}] = \theta.
$$

If an estimation is not unbiased, then the value of <em>bias</em> for estimation is defined as 

$$
b(\theta) \triangleq \mathbb{E}[\hat{\theta}] - \theta
$$

### Minimum variance criterion 

A natural choice for the optimality criterion is <em>Mean Squared Error (MSE)</em> defined as

$$
mse(\hat{\theta}) \triangleq \mathbb{E}[(\hat{\theta} - \theta)^2] = var(\hat{\theta}) + b^2(\theta)
$$

Unfortunately, adoption of this natural choice leads to unrealizable estimators, ones that cannot be written solely as a function of data since it depends on bias. Hence, from a practical point of view, MSE estimators are useless. An alternative approach is to constrain the bias to zero and minimize the variance. Such an estimation is called <strong><em>Minimum Variance Unbiased (MVU)</em></strong> estimation. Unfortunately, there is also no guarantee that an MVU exists. If MVU exists, there are a few techniques that could help us find it. 
* Finding the <em>Cramer Rao Lower Bound (CRLB)</em> and checking if some estimator can achieve it. 
* Applying the <em>Rao-Blackwell-Lehmann-Schaffe (RBLS)</em> theorem to an unbiased estimator. 
* Finding the <em>Best Linear Unbiased Estimator (BLUE)</em>.

CRLB is a lower bound for the variance of any unbiased estimator. If an estimator exists whose variance equals to that of CRLB <em>for any value of $\theta$</em>, then it must be the MVU estimator. In this case, the theory of CRLB immediately yields the estimator. It may happen that no estimator exists whose variance equals the bound, yet, MVU estimator may still exist. In such cases, we must resort to RBLS theorem to find the MVU. The third approach, BLUE, only works for particular datasets. 

## Cramer Rao Lower Bound (CRLB)
Assume that the PDF function $p(x,\theta)$ satisfies the regularity condition 

$$
\mathbb{E} \left[\frac{\partial \ln p(x,\theta)}{\partial \theta} \right] = 0,  \quad \forall \theta
$$

where the expectation is taken with respect to $p(x, \theta)$. Then the variance of any unbiased estimator $\hat{\theta}$ must satisfy 

$$
var(\hat{\theta}) \ge -\frac{1}{\mathbb{E}\left[ \frac{\partial^2 \ln p(x,\theta)}{\partial \theta^2}\right]} = \frac{1}{\mathbb{E}\left[ \left( \frac{\partial \ln p(x,\theta)}{\partial \theta}\right)^2 \right]}
$$

where the derivation is evaluated at the true value of $\theta$ and the expectation is taken with respect to $p(x, \theta)$. Furthermore, an unbiased estimator may be found that attains the bound for all $\theta$ if and only if 

$$
\frac{\partial \ln p(x, \theta)}{\partial \theta} = I(\theta) (g(x) - \theta)
$$

for some functions $g$ and $I$. The estimator which is the MVU estimator is $\hat{\theta} = g(x)$ and the minimum variance is $1/I(\theta)$.

An estimator which is unbiased and attains the CRLB is said to be <strong><em>efficient</em></strong>. The denominator of the lower bound in CRLB is called <strong><em>Fisher information</em></strong>. If it is desired to find the estimate for 

$$
\alpha = g(\theta)
$$

then the CRLB is proved to be 

$$
var(\hat{\alpha}) \ge -\frac{\left(\frac{\partial g}{\partial \theta}\right)^2}{\mathbb{E}\left[ \frac{\partial^2 \ln p(x,\theta)}{\partial \theta^2}\right]}
$$

This shows that the efficiency is destroyed by a non-linear transformation like $g(\theta)$ while it is maintained by linear transformations. 

{% highlight python %} 
pd.set_option('display.max_rows', None)
{% endhighlight %}

Useful resources:

[https://readthedocs.org/projects/pandas-datareader/downloads/pdf/latest/](https://readthedocs.org/projects/pandas-datareader/downloads/pdf/latest/)

