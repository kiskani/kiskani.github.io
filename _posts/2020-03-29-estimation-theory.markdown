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
Assume that the PDF function $p(x; \theta)$ satisfies the regularity condition 

$$
\mathbb{E} \left[\frac{\partial \ln p(x;\theta)}{\partial \theta} \right] = 0,  \quad \forall \theta
$$

where the expectation is taken with respect to $p(x; \theta)$. Then the variance of any unbiased estimator $\hat{\theta}$ must satisfy 

$$
var(\hat{\theta}) \ge -\frac{1}{\mathbb{E}\left[ \frac{\partial^2 \ln p(x; \theta)}{\partial \theta^2}\right]} = \frac{1}{\mathbb{E}\left[ \left( \frac{\partial \ln p(x;\theta)}{\partial \theta}\right)^2 \right]}
$$

where the derivation is evaluated at the true value of $\theta$ and the expectation is taken with respect to $p(x; \theta)$. Furthermore, an unbiased estimator may be found that attains the bound for all $\theta$ if and only if 

$$
\frac{\partial \ln p(x; \theta)}{\partial \theta} = I(\theta) (g(x) - \theta)
$$

for some functions $g$ and $I$. The estimator which is the MVU estimator is $\hat{\theta} = g(x)$ and the minimum variance is $1/I(\theta)$.

An estimator which is unbiased and attains the CRLB is said to be <strong><em>efficient</em></strong>. The denominator of the lower bound in CRLB is called <strong><em>Fisher information</em></strong>. If it is desired to find the estimate for 

$$
\alpha = g(\theta)
$$

then the CRLB is proved to be 

$$
var(\hat{\alpha}) \ge -\frac{\left(\frac{\partial g}{\partial \theta}\right)^2}{\mathbb{E}\left[ \frac{\partial^2 \ln p(x; \theta)}{\partial \theta^2}\right]}
$$

This shows that the efficiency is destroyed by a non-linear transformation like $g(\theta)$ while it is maintained by linear transformations. 

## Rao-Blackwell-Lehmann-Schaffe Theorem

RBLS Theorem allows us to find MVU estimator by a simple inspection of the PDF function. 

# Sufficient Statistics

A reasonable question to ask is which data samples are pertinent to the estimation problem? There may be many sufficient data sets to find the MVU. However, the one with the least number of elements is called the <strong>minimal sufficient statistic</strong>. The Neyman-Fisher theorem which is used to find the sufficient statistics is stated as follows. 

# Neyman-Fisher Factorization Theorem

If we can factor the pdf $p(x; \theta)$ as 

$$
p(x;\theta) = g(T(x), \theta) h(x)
\label{eq_fac}
$$

where $g$ is a function depending on $x$ only through $T(x)$ and $h$ is a function depending only on $x$, then $T(x)$ is a sufficient statistic for $\theta$. Conversely, if $T(x)$ is a sufficient statistic for $\theta$, then the pdf can be factored as in \eqref{eq_fac}.

Using the Neyman-Fisher theorem we can find sufficient statistics. Then we will use the RBLS theorem as to find MVU as follows. 

# RBLS Theorem

If $\check{\theta}$ is an unbiased estimator of $\theta$ and $T(x)$ is a sufficient statistic for $\theta$, then 

$$
\hat{\theta} = \mathbb{E}\left[ \check{\theta} | T(x) \right]
$$
 
is
* a valid estimator for $\theta$ (not depending on $\theta$)
* unbiased 
* of lesser or equal variance than that of $\check{\theta}$, for all $\theta$.

Additionally, if the sufficient statistic is <strong><em>complete</em></strong>, then $\hat{\theta}$ is the MVU estimator. A statistic is <em>complete</em> if there is only one function of the statistic that is unbiased. The property of completeness depends on the pdf of $x$, which in turn determines the pdf of the sufficient statistic. For many practical cases of interest this property holds. For instance, it holds for the exponential family of distributions. 

# Rao Blackwellization

Rao Blackwellization is a procedure to find the MVU estimator using the RBLS theorem. It involves the following steps:
1. Use Neyman-Fisher factorization theorem to find a single sufficient statistic $T(x)$ for $\theta$. 
2. Determine if the sufficient statistic is complete and if so, proceed. If not, this approach cannot be used to find MVU. 
3. Find a function $g$ of the sufficient statistic that yields an unbiased estimator $\hat{\theta} = g(T(x))$. This will be the MVU estimator. 

As an alternative implementation of step 3, we may evaluate 
$$
\hat{\theta} = \mathbb{E}\left[ \check{\theta} | T(x) \right]
$$, where $\check{\theta}$ is any unbiased estimator. 

## Best Linear Unbiased Estimator (BLUE)

The BLUE restricts the estimator to be linear in data 

$$
\hat{\theta} = \sum_{i=0}^{N-1} a_i x[i]
$$

where $a_i$'s are constants yet to be determined. The unbiased constraint is written as 

$$
\mathbb{E}[\hat{\theta}] = \sum_{i=0}^{N-1} a_i \mathbb{E}[x[i]] = \theta
$$

In order to satisfy this constraint, $\mathbb{E}[x[i]]$ must be linear in $\theta$ or 

$$
\mathbb{E}[x[i]] = s[i] \theta 
$$

where the $s[i]$'s are known. Therefore, the unbiased contraint can be written as 

$$
\mathbf{a}^T \mathbf{s} = 1
\label{eq_bias}
$$

We also have 

$$
var(\hat{\theta}) = \mathbb{E}\left[\mathbf{a}^T (\mathbf{x} - \mathbb{E}[\mathbf{x}])(\mathbf{x} - \mathbb{E}[\mathbf{x}])^T\mathbf{a} \right]= \mathbf{a}^T \mathbf{C} \mathbf{a}
\label{eq_var}
$$

If we minimize variance in \eqref{eq_var} subject to \eqref{eq_bias}, we can find the optimal coefficients $\mathbf{a}$ as 

$$
\mathbf{a}_{opt} = \frac{\mathbf{C}^{-1}\mathbf{s}}{\mathbf{s}^T\mathbf{C}^{-1}\mathbf{s}}
$$

Hence, the BLUE is 

$$
\hat{\theta} = \frac{\mathbf{s}^T\mathbf{C}^{-1}\mathbf{x}}{\mathbf{s}^T\mathbf{C}^{-1}\mathbf{s}}
$$

and the minimum variance is equal to

$$
var(\hat{\theta}) = \frac{1}{\mathbf{s}^T\mathbf{C}^{-1}\mathbf{s}}
$$

## Maximum Likelihood Estimation (MLE)

MLE is an estimation of the value of $\theta$ that maximizes the value of $p(x; \theta)$ the likelihood function for fixed $x$. The maximization is done over the range of all allowable values for $\theta$. It is used in situations that MVU does not exist or cannot be found even if it does exist. It is the most popular "turn-the-crank" estimation procedure. For most cases practical intereste it's performance is optimal for large enough data records. It is an estimator that is <strong>approximately optimal</strong>. It can be proved that as $N \to \infty$, the proposed estimator is unbiased and efficient. Hence, it is <strong>aymptotically unbiased</strong> and <strong>asymptotically efficient</strong>. More strongly, we can prove that if the pdf $p(x; \theta)$ satisfies some "regularity" conditions, then the MLE of the unknown parameter $\theta$ is asymptotically distributed (for large data records) according to 

$$
\hat{\theta} \overset{\text{a}}{\sim} \mathcal{N}(\theta, I^{-1}(\theta))
$$

where $I(\theta)$ is the Fisher information evaluated at the true value of the unknown parameter $\theta$. This shows that not only the estimator is asymptotically unbiased and can asymptotically achieve the CRLB (due to law of large numbers) but also it is following a Gaussian distribution.

The MLE estimation also follows <strong>invariance property</strong> which means that the MLE of the transformation of a parameter is found by substituting the MLE of the original parameter into the transformation operation. More precisely, the MLE of the parameter 

$$
\alpha = g(\theta)
$$

where the pdf $p(x; \theta)$ is parametrized by $\theta$, is given by 

$$
\hat{\alpha} = g(\hat{\theta})
$$

where $\hat{\theta}$ is the MLE of $\theta$ and $\hat{\theta}$ is found by maximizing $p(x;\theta)$. If $g$ is not a one-to-one function, then $\hat{\alpha}$ maximizes the modified likelihood function 

$$
p_T(x; \alpha) \triangleq \max_{\{\theta: \alpha = g(\theta)\}} p(x;\theta)
$$

<!---
## Bayesian Estimation 

In situations when MVU estimator cannot be found, we can use a Bayesian approach in which $\theta$ is considered to be a random variable as opposed to a deterministic value that we are interested in finding. 

Another very important property of MLE estimation is that MLE is equivalent to minimizing the KL distance between our estimate and the real data distribution. For a proof of this important fact, see [here](https://wiseodd.github.io/techblog/2017/01/26/kl-mle/).

{% highlight python %} 
pd.set_option('display.max_rows', None)
{% endhighlight %}

Useful resources:

[https://readthedocs.org/projects/pandas-datareader/downloads/pdf/latest/](https://readthedocs.org/projects/pandas-datareader/downloads/pdf/latest/)
--->
