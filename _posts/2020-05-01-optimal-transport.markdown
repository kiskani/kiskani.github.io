---
layout: post
title:  "Notes on Optimal Transport"
date:   2020-05-01 02:27:12 -0800
categories: optimaltransport
---

This post is dedicated to optimal transport. I will summarize the fundamentals of optimal transport in this page. Mainly, I will summarize the nice survery [Computational Optimal Transport](https://arxiv.org/abs/1803.00567) along with talks by [Marco Cuturi](https://www.youtube.com/watch?v=1ZiP_7kmIoc). 

## Monge Problem

The study of optimal transport first started by Monge in 1781 by trying to find a deterministic mapping $T(.)$ that can optimally transport all the mass in one space to another space. More formally, assuming that we have arbitrary probability measures $\alpha$ and $\beta$ on respective spaces of $\mathcal{X}$ and $\mathcal{Y}$, then Monge's problem is to find a mapping $T: \mathcal{X} \to \mathcal{Y}$ that minimizes

$$
\min_{T} \left\{  \int_{\mathcal{X}} c(x, T(x)) \mathrm{d} \alpha(x) ~~:~~ T_\sharp \alpha = \beta  \right\}
$$

where $c(.)$ is the transport cost function and $T_\sharp \alpha = \beta$ means that $T$ pushes forward the mass of $\alpha$ to $\beta$. This problem is of course diffult to solve and for over 200 years there has not been much progress on it. 

## Kantrovich relaxation 

In 1942, Kantrovich proposed to relax the deterministic nature of transportation problem. Instead of mapping any point $x$ to another point $T(x)$, he proposed to the mass at any point be potentially dispatched across several locations. In order to do this, he proposed to use joint probability distributions over the space $\mathcal{M}^1_{+} (\mathcal{X} \times \mathcal{Y})$ which loosely speaking denotes the space of positive measures that sum up to one. Defining 

$$
\mathcal{U}(\alpha, \beta) \triangleq \left\{ \pi \in \mathcal{M}^1_{+} (\mathcal{X} \times \mathcal{Y}) ~~:~~ P_{\mathcal{X} \sharp} \pi = \alpha ~~\mathrm{and}~~ P_{_\mathcal{Y}\sharp} \pi =\beta\right\}
$$

as the set of joint probability distributions with marginal distributions equal to $\alpha$ and $\beta$, then the Kantrovich relaxed problem would be of the form 

$$
\mathcal{L}_{c}(\alpha, \beta) \triangleq \min_{\pi \in \mathcal{U}(\alpha, \beta)} \int_{\mathcal{X} \times \mathcal{Y}} c(x,y) \mathrm{d} \pi(x,y) = \min_{(X,Y)} \left\{ \mathbb{E}_{(X,Y)}\left[ c(X,Y) \right] ~~:~~ X \sim \alpha, ~~ Y \sim \beta  \right\}.
$$

In case of discrete measures, defining

$$
\mathbf{U}(\mathbf{a}, \mathbf{b}) \triangleq \left\{ \mathbf{P} \in \mathbb{R}_+^{n \times m} ~~:~~ \mathbf{P} \mathbb{1}_m = \mathbf{a} ~~\mathrm{and} ~~ \mathbf{P}^T \mathbb{1}_n = \mathbf{b}\right\}
$$

the Kantrovich relaxation problem would be of the form

$$
\mathbf{L_C} (\mathbf{a}, \mathbf{b}) \triangleq \min_{\mathbf{P} \in \mathbf{U}(\mathbf{a}, \mathbf{b})} \sum_{i,j} \mathbf{C}_{i,j} \mathbf{P}_{i,j}
$$

where $\mathbf{C}_{i,j} = c(x_i, y_j)$. Notice that the solution of the Kantrovich problem results in a coupling that is localized along the Monge map $(x, T(x))$ as shown below (displayed in black).

![Kantrovich](kantrovich.png){:height="50%" width="50%"}

