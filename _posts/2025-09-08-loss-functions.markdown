---
layout: post
title:  "A deeper dive on loss functions"
date:   2025-09-08 00:00:00 -0000
categories: machinelearning
---

Let's go deeper into some of the loss functions in machine learning. We can start with cross entropy loss function. 

# Cross Entropy Loss Function

### Connection to Maximum Likelihood Estimation

Assume that we have i.i.d samples $D = \{(X_i, y_i)\}_{i=1}^N$ where $X_i$'s are features and $y_i$'s are binary labels of 0 or 1 and assume that we have a parametric prediction algorithm $f(. \mid \theta)$ that predicts the probability of the event that the label of each feature is 1, i.e. 

$$\hat{y_i} = f(X_i \mid \theta) = \mathrm{Pr} \big[y_i=1 \mid X_i, \theta \big]$$

 In Maximum Likelihood Estimation (MLE), we want to find the set of parameters $\theta$ for our model that maximize the probability of joint distributions of observed events. In other words, in MLE we want to maximize 

$$
\hat{\theta}_{\text{MLE}} = \arg\max_\theta P(D \mid \theta)
$$

while in MAP we want to instead maximize the likelihood of $$\theta$$ given $$D$$.

$$
  \hat{\theta}_{\text{MAP}} = \arg\max_\theta P(\theta \mid D) = \arg\max_\theta \big[ P(D \mid \theta) P(\theta) \big]
$$

Therefore, in MLE we have  

$$
\hat{\theta}_{\text{MLE}} = \arg \max_{\theta} L(D \mid \theta) = \arg \max_{\theta} \prod_{i=1}^N P(X_i, y_i \mid \theta) = \arg \max_{\theta} \prod_{i=1}^N P\big(y_i | X_i, \theta \big)  
$$ 

We can instead maximize the log of this likelihood function to find the maximizing set of parameters. Therefore, 

$$
\hat{\theta}_{\text{MLE}} = \arg \max_{\theta} \sum_{i=1}^N \log \big( P\big(y_i | X_i, \theta \big)\big)
$$

based on the definition in the first equation above, we can write

$$
P\big(y_i | X_i, \theta \big) = \hat{y_i}^{y_i} \big(1-\hat{y_i}\big)^{1-y_i}
$$

Therefore, the MLE problem reduces to solving the following equation 

$$
\hat{\theta}_{\text{MLE}} = \arg \min_{\theta} \sum_{i=1}^N -y_i \log(\hat{y_i}) -(1-y_i)\log(1-\hat{y_i}) = \arg \min_{\theta} CE(y, \hat{y})
$$

In other words, to do Maximum Likelihood Estimation we need to minimize the Cross Entropy loss function. 

### Connection to KL Divergence

For a given input $X_i$, let the true label distribution be

$$
p(y_i \mid X_i) =
\begin{cases}
p & \text{if } y_i=1,\\
1-p & \text{if } y_i=0,
\end{cases}
$$

and the model's predicted probability be

$$
q(y_i \mid X_i) =
\begin{cases}
q & \text{if } y_i=1,\\
1-q & \text{if } y_i=0.
\end{cases}
$$

Here $\(p \in [0,1]\)$ is the true Bernoulli parameter, and $\(q \in [0,1]\)$ is the model output (e.g. sigmoid). The Kullback–Leibler divergence from the true distribution $\(p\)$ to the model $\(q\)$ is

$$
\mathrm{KL}\big(p(\cdot\mid X_i)\,\|\,q(\cdot\mid X_i)\big)
= \sum_{y_i \in\{0,1\}} p(y_i \mid X_i)\,\log\frac{p(y_i \mid X_i)}{q(y_i \mid X_i)}.
$$

Using **entropy** and **cross-entropy**:

- Entropy of $\(p\)$:  
  $$
  H(p) = -\sum_{y_i} p(y_i)\log p(y_i).
  $$

- Cross-entropy between $\(p\)$ and $\(q\)$:  
  $$
  H(p,q) = -\sum_{y_i} p(y_i)\log q(y_i).
  $$

Thus,

$$
\mathrm{KL}(p\|q) = -H(p) + H(p,q).
$$

So we have

$$
\boxed{\mathrm{KL}(p\|q) = H(p,q) - H(p).}
$$

For a fixed true distribution $\(p\)$, the entropy $\(H(p)\)$ does **not** depend on the model $\(q\)$. Therefore,
**minimizing KL divergence with respect to  the model is equivalent to minimizing the cross-entropy with respect to the model**. 

$$
\arg\min_q \mathrm{KL}(p\|q) \;=\; \arg\min_q H(p,q).
$$

---

## Binary Classification (Observed Labels)
In practice, we observe samples \((x_i,y_i)\) with labels \(y_i\in\{0,1\}\).  
The per-sample **cross-entropy loss** is

\[
\ell_{\text{CE}}(q; y) = -\big(y\log q + (1-y)\log(1-q)\big).
\]

This is exactly

\[
-\log q(y),
\]

the negative log-probability the model assigns to the observed label.  
The expected cross-entropy over the true \(p\) equals \(H(p,q)\).

If the true label is deterministic (i.e. \(p\) is a delta at the observed \(y\)), then \(H(p)=0\), and

\[
\mathrm{KL}(p\|q) = H(p,q) = \ell_{\text{CE}}(q;y).
\]

Thus, minimizing KL is **exactly** minimizing the usual CE loss.

---

## Link to Maximum Likelihood
Cross-entropy (negative log probability of observed labels) is the **negative log-likelihood**.  

So minimizing average CE is equivalent to maximizing the likelihood (MLE):

\[
\text{minimize KL} 
\;\Longleftrightarrow\; \text{minimize CE} 
\;\Longleftrightarrow\; \text{maximize likelihood}.
\]

---

## Summary
Because

\[
\mathrm{KL}(p\|q)=H(p,q)-H(p),
\]

and \(H(p)\) is constant w.r.t. the model, minimizing KL w.r.t. \(q\) is equivalent to minimizing cross-entropy.  
In binary classification with observed labels, this reduces to the familiar **binary cross-entropy / negative log-likelihood loss**:

\[
-\big(y\log q + (1-y)\log(1-q)\big).
\]

