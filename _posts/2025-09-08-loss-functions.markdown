---
layout: post
title:  "A deeper dive on loss functions"
date:   2025-09-08 00:00:00 -0000
categories: machinelearning
---

Let's go deeper into some of the loss functions in machine learning. We can start with cross entropy loss function. 

# Cross Entropy Loss Function

- Connection to Maximum Likelihood Estimation

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
\hat{\theta}_{\text{MLE}} = \arg \max_{\theta} L(D \mid \theta) = \arg \max_{\theta} \prod_{i=1}^N f(X_i, y_i \mid \theta) = \arg \max_{\theta} \prod_{i=1}^N f\big(y_i | X_i, \theta \big)  
$$ 

set of parameters for this algorithm, we have

$$
\theta_{MLE} = \mathrm{Argmax}_{\theta} 
$$

Imagine that we have samples

- Connection to KL Divergence
