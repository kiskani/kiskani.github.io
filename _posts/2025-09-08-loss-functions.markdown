---
layout: post
title:  "A deeper dive on loss functions"
date:   2025-09-08 00:00:00 -0000
categories: machinelearning
---

Let's go deeper into some of the loss functions in machine learning. We can start with cross entropy loss function. 

# Cross Entropy Loss Function

- Connection to Maximum Likelihood Estimation

Assume that we have i.i.d samples $\{(X_i, y_i)\}_{i=1}^N$ where $X_i$'s are features and $y_i$'s are binary labels of 0 or 1 and assume that we have a prediction algorithm $f(.;\theta)$ that predicts the probability of the event that the label of each feature is 1, i.e. $\hat{y}_i = f(X_i;\theta) = \mathrm{Pr}\[y_i=1 | X_i, \theta\]$. In Maximum Likelihood Estimation (MLE), we want to find the set of parameters $\theta$ for our model that maximize the probability of joint distributions of observed events. In other words, we want to maximize 

$$
L(\theta) = \prod_{i=1}^N f(X_i, y_i; \theta) = \prod_{i=1}^N f(y_i | 
$$ 

set of parameters for this algorithm, we have

$$
\theta_{MLE} = \mathrm{Argmax}_{\theta} 
$$

Imagine that we have samples

- Connection to KL Divergence
