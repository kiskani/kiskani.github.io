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

## Kantrovich Relaxation 

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

## Wasserstein Distance

Consider the case of discrete probability distributions and let $n = m$ and for some $p \ge 1$, let $$\mathbf{C} = \mathbf{D}^p = \left[ \mathbf{D}_{i,j}^p \right]_{i,j} \in \mathbb{R}^{n \times n} $$, where $$\mathbf{D} \in \mathbb{R}_+^{n \times n}$$ is a distance on $$\mathbb{[} n \mathbb{]}$$, i.e.

* $$\mathbf{D} \in \mathbb{R}_+^{n \times n}$$ is symmetric. 
* $$\mathbf{D}_{i,j} = 0$$ if and only if $$i=j$$.
* $$\forall (i,j,k) \in \mathbb{[} n \mathbb{]}^3, ~~ \mathbf{D}_{i,k} \le \mathbf{D}_{i,j} + \mathbf{D}_{j,k}$$.

Then, in a non-trivial way, using Minkowski's inequality one can prove that 

$$
W_p(\mathbf{a}, \mathbf{b}) \triangleq \mathbf{L}_{\mathbf{D}^p}(\mathbf{a}, \mathbf{b})^{1/p},
$$

is indeed a distance. This metric is called <strong>Wasserstein Distance</strong>. Notice that for the case of $$0 < p \le 1$$, this is not necessarily true. In that case, $$\mathbf{D}^p$$ is itself a distance and therefore $$W_p(\mathbf{a}, \mathbf{b})^p$$ will be a distance on the simplex. In case of continuous domains, assuming $\mathcal{X} = \mathcal{Y}$ and $p \ge 1$ and $c(x,y) = d(x,y)^p$ for some distance $d$ on space $\mathcal{X}$, the <strong>$p$-Wasserstein</strong> distance on $\mathcal{X}$ is defined as 

$$
\mathcal{W}_p(\alpha, \beta) \triangleq \mathcal{L}_{d^p}(\alpha, \beta)^{1/p}
$$

and is similarly proved to be a distance metric. 


The Wasserstein distance $$\mathcal{W}_p$$ has many important properties, the most important being that it is a <em> weak distance </em>, i.e. it allows one to compare singular distributions (for instance, discrete ones) whose supports do not overlap and to quantify the spatial shift between the supports of two distributions. In particular, ``classical'' distances (or divergences) are not even defined between discrete distributions (the $L^2$ norm can only be applied to continuous measures with a density with respect to a base measure, and the discrete $\ell^2$ norm requires that positions $(x_i,y_j)$ take values in a predetermined discrete set to work properly). In sharp contrast, one has that for any $p> 0$, $$\mathcal{W}_p^p(\delta_x,\delta_y) = d(x,y)$$. Indeed, it suffices to notice that $$\mathcal{U}(\delta_x,\delta_y)=\{ \delta_{x,y}\}$$ and therefore the Kantorovich problem having only one feasible solution, $$\mathcal{W}_p^p(\delta_x,\delta_y)$$ is necessarily $$(d(x,y)^p)^{1/p}=d(x,y)$$. This shows that $$\mathcal{W}_p(\delta_x,\delta_y) \rightarrow 0$$ if $$x \rightarrow y$$. This property corresponds to the fact that $\mathcal{W}_p$ is a way to quantify [weak convergence](https://en.wikipedia.org/wiki/Convergence_of_measures).

A nice feature of the Wasserstein distance over a Euclidean space $$\mathcal{X}=\mathbb{R}^d$$ for the ground cost $$c(x,y)= \lVert x-y \lVert^2$$ is that one can factor out translations; indeed, denoting $$T_{\tau} : x \mapsto x-\tau$$ the translation operator, one has

$$
	\mathcal{W}_2(T_{\tau\sharp} \alpha,T_{\tau'\sharp} \beta)^2  =
	\mathcal{W}_2(\alpha,\beta)^2 - 2 \langle \tau-\tau', \mathbf{m}_{\alpha}-\mathbf{m}_{\beta}  \rangle + \lVert \tau-\tau' \lVert^2,
$$

where $$\mathbf{m}_{\alpha} \triangleq \int_\mathcal{X} x \mathrm{d}\alpha(x) \in \mathbb{R}^d$$ is the mean of $\alpha$. In particular, this implies the nice decomposition of the distance as

$$
	\mathcal{W}_2(\alpha,\beta)^2  = \mathcal{W}_2(\tilde\alpha,\tilde{\beta})^2  + \lVert \mathbf{m}_{\alpha}-\mathbf{m}_{\beta} \lVert^2,
$$

where $(\tilde\alpha,\tilde{\beta})$ are the ``centered'' zero mean measures $\tilde\alpha=T_{\mathbf{m}_{\alpha}\sharp}\alpha$.

In case of $$p = +\infty$$, the limit of $\mathcal{W}_p^p$ as $$p \rightarrow +\infty$$ is

$$
	\mathcal{W}_{\infty}(\alpha,\beta) \triangleq
	\min_{\pi \in \mathcal{U}(\alpha,\beta)} \sup_{(x,y) \in \mathrm{Supp}(\pi)} d(x,y),
$$

where the sup should be understood as the essential supremum according to the measure $\pi$ on $\mathcal{X}^2$. In contrast to the cases $p<+\infty$, this is a nonconvex optimization problem, which is difficult to solve numerically and to study theoretically. The $\mathcal{W}_{\infty}$ distance is related to the Hausdorff distance between the supports of $(\alpha,\beta)$.

## Kantrovich Duality

The Kantorovich problem is a constrained convex minimization problem, and as such, it can be naturally paired with a so-called dual problem, which is a constrained concave maximization problem. The dual problem is of the form

$$
	\mathbf{L}_{\mathbf{C}}(\mathbf{a},\mathbf{b}) = \max_{(\mathbf{f},\mathbf{g}) \in \mathbf{R}(\mathbf{C})} \langle \mathbf{f}, \mathbf{a} \rangle + \langle \mathbf{g}, \mathbf{b} \rangle,
$$

where the set of admissible dual variables is

$$
    \mathbf{R}(\mathbf{C}) \triangleq \{ (\mathbf{f},\mathbf{g}) \in \mathbb{R}^n \times \mathbb{R}^m ~:~  \mathbf{f} \mathbb{1}_m^T + \mathbb{1}_n \mathbf{g} \leq \mathbf{C} \}.
$$

Such dual variables are often referred to as <strong> Kantorovich potentials. </strong> This result is a direct consequence of the more general result on the strong duality for linear programs and can also be achieved using Lagrangian duality. Kantrovich dual problem in case of continuous distributions will be of the form

$$ \mathcal{L}_c(\alpha,\beta) = \sup_{(f,g) \in \mathcal{R}(c)}\ \int_\mathcal{X} f(x) \mathrm{d}\alpha(x) + \int_\mathcal{Y} g(y) \mathrm{d}\beta(y),
$$

where the set of admissible dual potentials is

$$\mathcal{R}(c) \triangleq \{(f,g) \in \mathcal{C}(\mathcal{X}) \times \mathcal{C}(\mathcal{Y}) ~~~~ \forall (x,y),~ f(x)+g(y) \leq c(x,y)\}. $$

Here, $(f,g)$ is a pair of continuous functions and are also called, as in the discrete case, ``Kantorovich potentials.'' The primal-dual optimality allows us to track the support of the optimal plan as follows

$$
\mathrm{Supp}(\pi) \subset \{(x,y) \in \mathcal{X} \times \mathcal{Y} ~:~ f(x) + g(y) = c(x,y) \}
$$

and in case of discrete measures 

$$
\{(i,j) \in \mathbb{[} n \mathbb{]} \times \mathbb{[} m \mathbb{]} ~:~ \mathbf{P}_{i,j} > 0 \} \subset \{(i,j) \in \mathbb{[} n \mathbb{]} \times \mathbb{[} m \mathbb{]} ~:~ \mathbf{f}_i + \mathbf{g}_j = \mathbf{C}_{i,j} \}
$$

Note that in contrast to the primal problem, showing the existence of solutions to the dual problem is nontrivial, because the constraint set $\mathcal{R}(c)$ is not [compact](https://en.wikipedia.org/wiki/Compact_space) and the function to minimize noncoercive. However, in the case $c(x,y) = d(x,y)^p$ with $p \ge 1$, one can, however, show that optimal $(f,g)$ are necessarily Lipschitz regular, which enables us to replace the constraint by a compact one.

