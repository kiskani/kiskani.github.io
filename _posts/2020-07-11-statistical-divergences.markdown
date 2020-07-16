---
layout: post
title:  "Statistical Divergences"
date:   2020-07-11 09:56:12 -0800
categories: machinelearning
---

This is a brief review of statistical divergences as explained in chapter 8 of [Computational Optimal Transport](https://arxiv.org/abs/1803.00567). A <strong>divergence</strong> $D$ satisfies the following properties:
* $D(\alpha, \beta) \ge 0$. 
* $D(\alpha, \beta) = 0 \Longleftrightarrow \alpha = \beta$

However, unlike a distance it does not have to be symmetric or satisfy the traingle inequality. Divergences compare two input measures by comparing their masses <em>pointwise</em>, without introducing any notion of mass transportation. Divergences are functionals which, by looking at the pointwise ratio between two measures, give a sense of how close they are. They have nice analytical and computational properties and build  upon <em>entropy functions</em>.

* If ${\displaystyle X}$ is a topological space, ${\displaystyle x_{0}}$ is a point in ${\displaystyle X}$ and ${\displaystyle f\colon X\to \mathbb {R} \cup \\{-\infty ,\infty \\}}$ is an extended real-valued function. We say that ${\displaystyle f}$ is <strong>lower semi-continuous</strong> at ${\displaystyle x_{0}}$ if for every ${\displaystyle y<f(x_{0})}$ there exists a neighborhood ${\displaystyle U}$ of ${\displaystyle x_{0}}$ such that ${\displaystyle y<f(x)}$ for all ${\displaystyle x\in U}$. For the particular case of a metric space, this can be expressed as

$${\displaystyle \liminf _{x\to x_{0}}f(x)\geq f(x_{0})}$$

&nbsp; &nbsp; &nbsp; &nbsp; The function ${\displaystyle f}$ is called lower semi-continuous if it is lower semi-continuous at every point of its 
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp;  domain. A function is lower semi-continuous if and only if ${\displaystyle \\{x\in X:~f(x)>y \\}}$ is an open set for 
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; every ${\displaystyle y\in \mathbb {R} }$. Alternatively, a function is lower semi-continuous if and only if all of its lower level 
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  sets ${\displaystyle \\{x\in X:~f(x)\leq y\\}}$ are closed. Lower level sets are also called sublevel sets or trenches.

* If ${\displaystyle X}$ is a topological space, ${\displaystyle x_{0}}$ is a point in ${\displaystyle X}$ and ${\displaystyle f\colon X\to \mathbb {R} \cup \\{-\infty ,\infty \\}}$ is an extended real-valued function. We say that ${\displaystyle f}$ is <strong>upper semi-continuous</strong> at ${\displaystyle x_{0}}$ if for every ${\displaystyle y>f(x_{0})}$ there exists a neighborhood ${\displaystyle U}$ of ${\displaystyle x_{0}}$ such that ${\displaystyle f(x)<y}$ for all ${\displaystyle x\in U}$. For the particular case of a metric space, this can be expressed as

$${\displaystyle \limsup _{x\to x_{0}}f(x)\leq f(x_{0})}$$

&nbsp; &nbsp; &nbsp; &nbsp;  The function ${\displaystyle f}$ is called upper semi-continuous if it is upper semi-continuous at every point of its &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;    domain. A function is upper semi-continuous if and only if ${\displaystyle \\{x\in X:~f(x)<y\\}}$ is an open set &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp;  for every ${\displaystyle y\in \mathbb {R} }$.

* A function is continuous at $x_0$ if and only if it is both upper and lower semi-continuous there. 

* A function $\phi : \mathbb{R} \to \mathbb{R} \cup \\{\infty\\}$ is an <strong>entropy function</strong> if it is lower semicontinuous, convex, $\mathrm{dom} ~\phi\subset [0,\infty)$, and satisfies the following feasibility condition:  $\mathrm{dom} ~ \phi \; \cap   (0, \infty) \neq \emptyset$. The speed of growth of $\phi$ at $\infty$ is described by 

$$\phi'_\infty = \lim_{x\rightarrow +\infty} \frac{\varphi(x)}{x} \in \mathbb{R} \cup \{\infty\} \, .$$

&nbsp; &nbsp; &nbsp; &nbsp; If $\phi'_\infty = \infty$, then $\phi$ grows faster than any linear function and $\phi$ is said <em>superlinear</em>. Any  entropy 
&nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp;  function $\phi$ induces a $\phi$-divergence or $f$-divergence) as defined below.

* Let $\phi$ be an entropy function. For $\alpha,\beta \in \\mathcal{M}(\mathcal{X})$, let $\frac{\mathrm{d} \alpha}{\mathrm{d} \beta} \beta + \alpha^{\perp}$ be the Lebesgue decomposition of $\alpha$ with respect to $\beta$. The <strong>divergence</strong> $\mathcal{D}_\phi$ is defined by

$$	\mathcal{D}_\phi (\alpha|\beta) \triangleq \int_X \phi\left(\frac{\mathrm{d} \alpha}{\mathrm{d} \beta} \right) \mathrm{d} \beta + \phi'_\infty \alpha^{\perp}(X)$$

&nbsp; &nbsp; &nbsp; &nbsp; if $\alpha,\beta$ are nonnegative and $\infty$ otherwise. 

* In case of discrete measures $\alpha = \sum_i a_i \delta_{x_i}$ and $\beta = \sum_i b_i \delta_{x_i}$ which are supported on the same set of $n$ points $(x_i)_{i=1}^n \subseteq \mathcal{X}$ the <strong>$\phi$-divergence</strong> is defined as 

$$\mathcal{D}_\phi(\mathbf{a}|\mathbf{b}) = \sum_{i \in \mathrm{Supp}(\mathbf{b})} \phi\left({ \frac{a_i}{b_i} } \right) b_i + \phi'_\infty \sum_{i \notin \mathrm{Supp}(\mathbf{b})} a_i,$$

&nbsp; &nbsp; &nbsp; &nbsp; where $$\mathrm{Supp}(b) \triangleq \{i \in \mathbb{[} n \mathbb{]} ~:~ b_i \neq 0 \}$$.

* Many divergences can be written as $\phi$-divergence. Here are a few examples:

| Name        | $$D_f(P \lVert Q)$$ | Generator $\phi$     | 
| :---        |    :----:   |          :---: | 
| KL-divergence      | $\int p(x) \log \left( \frac{p(x)}{q(x)} \right) \mathrm{d}x$      | ${\displaystyle t\log t}$  | 
| reverse KL-divergence   | $\int q(x) \log \left( \frac{p(x)}{q(x)} \right) \mathrm{d}x$         | ${\displaystyle -\log t}$     |
|squared Hellinger distance | $\int \left( \sqrt{p(x)} - \sqrt{q(x)} \right) ^2 \mathrm{d} x$ | $({\sqrt  {t}}-1)^{2},\,2(1-{\sqrt  {t}})$ |
|Total variation distance | $\frac{1}{2} \int \lvert p(x) - q(x) \rvert \mathrm{d} x$ | $\frac{1}{2} \lvert t - 1 \rvert$ |
|Pearson $\chi ^{2}$-divergence | $\int \frac{(p(x) - q(x))^2}{p(x)}\mathrm{d} x$ | ${\displaystyle (t-1)^{2},\,t^{2}-1,\,t^{2}-t}$ |
| Neyman $\chi ^{2}$-divergence (reverse Pearson) | $\int \frac{(p(x) - q(x))^2}{q(x)}\mathrm{d} x$  |	${\displaystyle {\frac {1}{t}}-1,\,{\frac {1}{t}}-t}$ |
{:.mbtablestyle}