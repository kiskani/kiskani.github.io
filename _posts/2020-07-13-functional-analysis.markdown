---
layout: post
title:  "A Recap of Functional Analysis"
date:   2020-07-9 10:57:12 -0800
categories: analysis
---

This is a recap on basic definitions and theorems in functional analysis. Mainly from a youtube course taught by [Claudio Landim](https://www.youtube.com/watch?v=OonaUALrKUk&list=PLo4jXE-LdDTTIIIRwqK35CbFJieSJEcVR).

* If $X$ is a [linear space](https://en.wikipedia.org/wiki/Vector_space) (or vector space), $Y \subseteq X$ is a <strong>linear subspace</strong> if $\forall x, y \in Y$ and $\forall \alpha \in \mathbb{K}$ where $\mathbb{K}$ is the underlying field, we have $\alpha x + y \in Y$.

* If $$\{Y_{\theta} ~:~ \theta \in I\}$$ is a family of linear subspaces in $X$, then $$\bigcap_{\theta \in I} Y_{\theta}$$ is a linear subspace of $X$.

* A family of sets $$\{Y_{\theta} ~:~ \theta \in I\}$$ is <strong>totally ordered</strong> if $\forall \theta_1, \theta_2 \in I$ either $Y_{\theta_1} \subseteq Y_{\theta_2}$ or $Y_{\theta_2} \subseteq Y_{\theta_1}$.

* If $$\{Y_{\theta} ~:~ \theta \in I\}$$ is a family of linear subspaces in $X$ which is totally ordered, then $$\bigcup_{\theta \in I} Y_{\theta}$$ is a linear subspace of $X$.

* Let the set $S \subseteq X$ be a subset of a linear space, then the <strong>linear span</strong> of $S$ denoted by $LS(S)$ is the defined as the intersection of all linear subspaces of $X$, i.e. $$\bigcap Y_{\theta}$$ where $Y_{\theta}$ is a linear subspace of $X$. It can then be proved that the linear span of $S$ is the smallest linear subspace which contains $S$. 

* It can be proved that the linear span of $S$ is formed by considering all linear combinations of elements of $S$, i.e. $$LS(S) = \left\{ \sum_{j=1}^n \alpha_j x_j, n \ge 1, \alpha_j \in \mathbb{K}, x_j \in S \right\}$$. This shows that all the elements of the linear span can be written as the finite linear combination of the elements of $S$. 

* Let $Y$ be a linear subspace of a set $X$ over a field $\mathbb{K}$, then we can prove that the relation $\sim$ defined as $x \sim y$ if $x - y \in Y$ is an equivalence relation. If we define $[x] = \\{ y \in X ~:~ y \sim x\\}$, then the <strong>quotient space of $X$ modulo $Y$</strong> is defined as $$X \lvert Y \triangleq \{[x] ~:~ x \in X\}$$. It can be proved that the summation $[x]+[y] = [x + y]$ and the multiplication $\alpha [x] = [\alpha x]$ are well defined and the quotient space is a linear space under this summation and multiplication. 

* Let $X^{\star}, X$ be two spaces over a field $\mathbb{K}$, a map $T : X \to X^{\star}$ is a <strong>linear map</strong> if $\forall x, y \in X$ we have $T(x+y) = T(x) + T(y)$ and $\forall x \in X, \forall \alpha \in \mathbb{K}$ we have $T(\alpha x) = \alpha T(x)$. Linear spaces $X^{\star}$ and $X$ are <strong>isomorph</strong> if there exists a linear map $T : X \to X^{\star}$ which is a bijection. 

* If $X$ is a space and $T  : X \to X^{\star}$ is a linear map from $X$ and $Y \subseteq X$ is a linear subspace of $X$ and $Y^{\star} \subseteq X^{\star}$ is a linear subspace of $X^{\star}$,  then 
	- $$T(Y) = \{T(x) ~:~ x \in Y \}$$ is a linear subspace.
	- $$T^{-1}(Y^{\star}) = \{ x \in X ~:~ T(x) \in Y^{\star} \}$$ is a linear subspace.

* Any linear subspace is a convex set. 

* If $Y_1$ and $Y_2$ are convex subsets of a set $X$, then $$Y_1 + Y_2 = \{y_1+y_2 ~:~ y_1 \in Y_1, y_2 \in Y_2~ \}$$ is a convex set. 

* If $$\{Y_{\theta} ~:~ \theta \in I\}$$ is a family of convex sets, then then $$\bigcap_{\theta \in I} Y_{\theta}$$ is a convex set.

* If $$\{Y_{\theta} ~:~ \theta \in I\}$$ is a totally ordered family of convex sets, then then $$\bigcup_{\theta \in I} Y_{\theta}$$ is a convex set. 

* If $X$ is a space and $T  : X \to Y$ is a linear map and $K \subseteq X$ is a convex set, then $T(K)$ is a convex set. 

* If $X$ is a space and $T  : X \to Y$ is a linear map and $K \subseteq Y$ is a convex set, then $T^{-1}(K)$ is a convex set. 

* If $S \subset X$, and $$\{K_{\theta} ~:~ \theta \in I\}$$ is the family of all convex sets that contain $S$, i.e. $S \subseteq K_{\theta}$, then <strong>convex hull</strong> of $S$ is the intersetion of all these convex sets, i.e.  $$cv(S) \triangleq \bigcap_{\theta \in I} K_{\theta}$$. It can be proved that the convex set is the smallest convex set which contains $S$. Further it can be proved that the convex hull of $S$ is equal to the set of all convex combinations of elements of $S$, i.e. $$cv(S) = \left\{ \sum_{j=1}^n \alpha_j x_j ~:~ n \ge 1, x_j \in S, \alpha_j \in [0,1], \sum_{j=1}^n {\alpha_j} =1 \right\}$$.