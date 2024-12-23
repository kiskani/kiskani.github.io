---
layout: post
title:  "Batch Normalization"
date:   2025-01-03 00:00:00 -0000
categories: machinelearning
---

In this page, I will talk about few tips related to batch normalization in neural networks. Main reference for this page is the original [batch normalization paper](https://arxiv.org/pdf/1502.03167).

# Batch normalization

* The main goal in batch normalization is to prevent the internal covariate shift within the layers of the network by restricting the changes to each layer to a specific range. This allows the parameters in the previous layers of the network to change without significant changes to the distribution of the inputs to subsequent layers. Complete normalization of the inputs to each layer is an expensive operation and requires computing the covariate matrix. Alternatively, batch normalization is a differentiable transformation that does not require the analysis of the entire training set after every parameter update. Since the input distribution of each layer does not change significantly when we use batch normalization, it significantly speeds up the training process of neural networks while boosting the performance of it.

* Batch normalization makes two simplications to achieve the goal of normalizing data. First the ``whitening'' process is done for each feature independently of all the other features to make it 0 have a 0 mean and variance of 1. Secondly, batch normalization uses mini-batches in stochastic gradient training, each mini-batch produces estimates of the mean and variance of each activation.

* Normalizing the $k$-th feature dimension of the $d$-dimensional input $x=(x^{1}, \dots, x^{d})$ as 

$$
\hat{x}^k = \frac{x^k - \mathbb{E}[x^k]}{\sqrt{\mathrm{VAR}[x^k]}}
$$

is known to speed-up the convergence even when features are not decorrelated. However, this may change what the layer can represent. To address this, we make sure that the transformation inserted in the network can represent the identity transform. To accomplish this, for each feature dimension $x^k$ we introduce a pair of parameters $\gamma^k, \beta^k$ which scale and shift the normalized value:

$$
y^k = \gamma^k \hat{x}^k + \beta^k
$$

These parameters are learned along with the original model parameters, and restore the representation power of the network. Algorithm 1 shows how batch normalization is done over a mini-batch. 

![Algorithm 1](alg1-bn.png) 

* **The scaled and shifted values $y$ are passed to other network layers**. The normalized activations $\hat{x}$ are **internal** to our transformation, but their presence is crucial. The distributions of values of any $\hat{x}$  has the expected value of 0 and the variance of 1, as long as the elements of each mini-batch are sampled from the same distribution. These sub-network inputs all have fixed means and variances, and although the joint distribution of these normalized $\hat{x}^k$ can change
over the course of training, we expect that the introduction of normalized inputs accelerates the training of the
sub-network and, consequently, the network as a whole. The learned affine transform applied to these normalized
activations allows the BN transform to represent the identity transformation and preserves the network capacity.





