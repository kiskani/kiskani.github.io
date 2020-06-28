---
layout: post
title:  "Notes on Mathematical Analysis"
date:   2020-06-25 10:57:12 -0800
categories: analysis
---

This is a brief primer on basic definitions and theorems in mathematical analysis. These important results are from [Integration and Modern Analysis](https://www.amazon.com/Integration-Modern-Analysis-John-Benedetto/dp/0817643060) and [Topology](https://www.amazon.com/Topology-Munkres-International-Economy-James/dp/B01DVPYWG8/). 

## Definitions

* A <strong> group </strong> is a set $G$ along with a rule $$\odot$$ for combining elements $a, b$ of $G$ such that 
	- <em>Closure</em>: For all $a, b$ in $G$ we have $a \odot b \in G$. 
	- <em>Associativity</em>: For all $a, b, c$ in $G$ we have $(a \odot b) \odot c = a \odot (b \odot c)$.
	- <em>Identity element</em>: There exists a unique element $e$ in $G$ such that, for every element $a$ in $G$, the equation $e \odot a = a \odot e = a$ holds. 
	- <em>Inverse</em>: For each $a$ in $G$, there exists an element $b$ in $G$, commonly denoted $a^{-1}$ such that $a \odot b = b \odot a = e$, where $e$ is the identity element.

* A <strong> field </strong> is a set $F$ together with two binary operations on $F$ called <em>addition (+)</em> and <em>multiplication (.)</em>. A binary operation on $F$ is a mapping $F \times F \to F$, that is, a correspondence that associates with each ordered pair of elements of $F$ a uniquely determined element of $F$. These operations are required to satisfy the following properties, referred to as <strong>field axioms</strong>. In these axioms, $a, b$, and $c$ are arbitrary elements of the field $F$.
	- <em>Associativity of addition and multiplication:</em> $a + (b + c) = (a + b) + c$, and $a · (b · c) = (a · b) · c$.
	- <em>Commutativity of addition and multiplication:</em> $a + b = b + a$, and $a · b = b · a$.
	- <em>Additive and multiplicative identity:</em> there exist two different elements 0 and 1 in $F$ such that $a + 0 = a$ and $a · 1 = a$.
	- <em>Additive inverses:</em> for every a in $F$, there exists an element in $F$, denoted $−a$, called the additive inverse of $a$, such that $a + (−a) = 0$.
	- <em>Multiplicative inverses:</em> for every $a \neq 0$ in $F$, there exists an element in $F$, denoted by $a^{−1}$, called the multiplicative inverse of $a$, such that $a · a^{-1} = 1$.
	- <em>Distributivity of multiplication over addition:</em> a · (b + c) = (a · b) + (a · c).

* A <strong> vector space</strong> $V$ over a field $F$ is a set together with two laws of composition. 
	- <em> addition:</em> $V \times V \to V$, written as $v, w \rightsquigarrow v+w$ for $v$ and $w$ in $V$.
	- <em> scalar multiplication by elements of the field:</em> $F \times V \to V$ written as $c, w \rightsquigarrow cw$ for $c \in F$ and $w \in V$.

* Let $X$ be a vector space over $\mathbb{F}$ where $\mathbb{F} = \mathbb{R}$ or $\mathbb{F} = \mathbb{C}$. We call $X$ a <strong>normed vector space   </strong> if there is a function $$\lVert .\rVert: X \to \mathbb{R}^{+}$$ such that 
	- $$\forall x \in X, ~ \lVert x \rVert = 0 \Longleftrightarrow x = 0$$.
	- $$\forall x, y \in X, ~ \lVert x + y \rVert \le \lVert x \rVert + \lVert y \rVert$$.
	- $$\forall a \in F, \forall x \in X, ~ \lVert a x \rVert = \lvert a \rvert \lVert x \rVert $$.

* Limit Superior and Limit Inferior for a sequence $x_n$ are defined as 

$$\overline{\lim_{n \to \infty}} x_n \triangleq \inf_n \sup_{k \ge n} x_k $$

$$\underline{\lim_{n \to \infty}} x_n \triangleq \sup_n \inf_{k \ge n} x_k $$

* A function $f: A \to B$ is <strong> surjective (onto) </strong> if every element of $B$ is the image of some element $A$ under the function $f$. It is <strong> injective (one-to-one)</strong> if for each pair of distinct points of $A$, their images under $f$ are different. And it is <strong> bijective </strong> if it is both injective and surjective. 

* A <strong>topology</strong> on a set $X$ is a collection $\tau$ of subsets of $X$ having the following properties:
	- $\emptyset$ and $X$ are in $\tau$.  
	- The union of elements of any subcollection of $\tau$ is in $X$.
	- The intersection of the elements of any <em>finite</em> subcollection of $\tau$ is in $X$. 

* A set $X$ for which topology $\tau$ has been identified is called a <strong> topological space. </strong>

* A subset $U \subseteq X$ is an <strong> open set </strong> of $X$ if $U$ belongs to the collection $\tau$. If $U$ is an open set containing $x$, then $U$ is called to be a <strong>neighborhood</strong> of $x$.

* If $\tau_1$ and $\tau_2$ are two topologies on a given set $X$, and $\tau_1 \subseteq \tau_2$, then $\tau_2$ is a <strong> finer </strong> topology than $\tau_1$ and $\tau_1$ is <strong> coarser </strong> than $\tau_2$.

* A topological space $X$ is called a <strong> Hausdorff space</strong> if for every pair $x_1, x_2$ of distinct points of $X$, there exist neighborhoods $U_1$ and $U_2$ of $x_1$ and $x_2$, respectively, that are disjoint. This condition is a relatively strong condition and a looser condition which is called the <strong> $T_1$ axiom </strong> requires that finite point sets be closed. Every finite point set in a Hausdorff space is closed. 

* If $X$ and $Y$ are topological spaces, a function $f: X \to Y$ is said to be <strong>continuous</strong> if for each open subset $V$ of $Y$, the set $f^{-1}(V)$ is an open set of $X$. If $f$ is bijective and both $f$ and $f^{-1}$ are continuous, then $f$ is called a <strong>homeomorphism</strong>.

* If $X$ is a topological space, it is said to be <strong> metrizable </strong> if there exists a metric $d$ on the set $X$ that induces the topology of $X$. A <strong> metric space</strong> is a metrizable space, together with a specific metric $d$ that gives the topology of $X$. 

* A collection $\mathcal{A}$ of subsets of a space $X$ is said to <strong> cover </strong> $X$ or to be a <strong> covering </strong> of $X$, if the union of the elements of $\mathcal{A}$ is equal to $X$. It is called an <strong> open covering </strong> of $X$ if its elemets are open subsets of $X$.

* A space $X$ is said to be <strong> compact </strong> if every open covering of $X$ contains a finite subcollection that covers $X$.

* A function $f$ from the metric space $(X, d_x)$ to the metric space $(Y, d_y)$ is said to be <strong> uniformly continuous </strong> if given $\epsilon > 0$, there exists $\delta > 0$ such that for every pair of points $x_0, x_1$ of $X$ such that $d_x(x_0, x_1) < \delta$ we have $d_y(f(x_0), f(x_1)) < \epsilon$. 

* If $X$ is a space, a point $x \in X$ is said to be an <strong> isolated point </strong> of $X$ if the one-point set $\\{x\\}$ is open in $X$.

* Given a subset $A$ of a topological space $X$, the <strong> interior </strong> of $A$ denoted by <strong>Int $A$</strong> is defined as the union of all open sets contained in $A$. The <strong>closure</strong> of $A$ denoted by <strong> CI $A$ </strong> or $\bar{A}$ is defined as the intersection of all closed sets containing A. We have 

$$\mathrm{Int} A \subseteq A \subseteq \bar{A} $$

* A space $X$ is said to be <strong> limit point compact </strong> if every infinite subset of $X$ has a limit point. 

* A space $X$ is said to be <strong> sequentially compact </strong> if every sequence of points of $X$ has a convergent subsequence. 

* A space $X$ is said to be <strong> locally compact at point $x$ </strong> if there is some compact subspace $C$ of $X$ that contains a neighborhood of $x$. If space $X$ is locally compact at each of its points, $X$ is said to be <strong> locally compact.</strong> The real line $\mathbb{R}$ is locally compact while the subspace $\mathbb{Q}$ of rational numbers is not locally compact. Also $\mathbb{R}^n$ is locally compact. 

* A sequence $\\{x_n : n = 1, \dots \\} \subseteq X$ where $X$ is a metric space with metric $\rho$ is called <strong>Cauchy</strong> if $\forall \epsilon > 0, \exists N$ such that $\forall m,n > N$, we have $\rho(x_m, x_n) < \epsilon$.  

* If $X$ is a metric space in which every Cauchy sequence $\\{x_n: n=1,\dots\\}$ converges to some element $x \in X$, i.e. $\rho(x_n,x) \to 0$, then $X$ is called <strong>complete</strong>.

* A complete normed vector space is called a <strong> Banach space</strong>.

* Let $$\mathbb{F} = \mathbb{R}$$ or $$\mathbb{F} = \mathbb{C}$$. A <strong>Hilbert space</strong> $H$ is a Banach space with a function $\langle .,. \rangle : H \times H \to \mathbb{F}$ that satisfies the following conditions:
	- $\forall x,y \in H$, we have $\langle x,y \rangle = \langle y,x \rangle$
	- $\forall x,y,z \in H$, we have $\langle x+y,z \rangle = \langle x, z \rangle + \langle y, z \rangle$
	- $\forall a \in \mathbb{F}, \forall x,y \in H$, we have $\langle ax,y \rangle = a\langle x,Y \rangle$
	- $\forall x \in H$, we have $\lVert x \rVert = \sqrt{\langle x,x \rangle}$

* If $$\mathcal{P}(X)$$ denotes the power set of a set $X$, then a collection of sets $$\mathcal{R} \subseteq \mathcal{P}(X)$$ is called a <strong>ring</strong> if $$\forall A, B \in \mathcal{R}$$ we have that $$A \cap B \in \mathcal{R}$$, $$A \cup B \in \mathcal{R}$$ and $$A \setminus B \in \mathcal{R}$$.

* A ring is a <strong>$\sigma$-ring</strong> if for any sequence $$\{A_n : n=1,\dots\}$$ we have that $$\bigcap_{n=1}^{\infty} A_n \in \mathcal{R}$$ and $$\bigcup_{n=1}^{\infty} A_n \in \mathcal{R}$$.

* A ring (or $\sigma$-ring) $$\mathcal{A}$$ over a set $X$ is called an <strong> algebra </strong> (or <strong>$\sigma$-algebra</strong>) if $$X \in \mathcal{A}$$. In this case, $$\emptyset \in \mathcal{A}$$ and $$A \in \mathcal{A}$$ implies that $$A^c \in \mathcal{A}$$. 

* We say that a collection $$\mathcal{S} \subseteq \mathcal{P}(X)$$ is a <strong>semiring</strong> if,
	- $\forall A, B \in \mathcal{S}$, we have $A \cap B \in \mathcal{S}$. 
	- $\forall A, B \in \mathcal{S}$, we have $$A \setminus B = \bigcup_{n=1}^{n} A_j$$ for some pairwise disjoint sequence $$\{A_1,\dots, A_n\} \subseteq \mathcal{S}$$. This condition is equivalent to saying that $\forall A \in \mathcal{S}$, $A^c$ can be written as a finite union of disjoint elements of $\mathcal{S}$, i.e. $$A^c = \bigcup_{j=1}^{n} A_j$$.

* If $$\mathcal{S} \subseteq \mathcal{P}(X)$$ is a semiring and $X \in \mathcal{S}$, then $\mathcal{S}$ is called a <strong>semialgebra</strong>. Notice that if $\mathcal{Q}$ is an algebra, then it is also a semialgebra. 

* For a collection $\mathcal{C} \in \mathcal{P}(X)$, the <strong>  algebra (or $\sigma$-algebra) generated by $\mathcal{C}$</strong> is the smallest algebra (or $\sigma$-algebra) that contains  $\mathcal{C}$. This algebra (or $\sigma$-algebra) always exists. 

* We say that a sequence of sets $\\{A_j : j=1,...\\}$ is a <strong> decreasing sequence </strong> denoted by $$A_n \downarrow A$$, if $A_j \subseteq A_{j-1}$ for each $j \ge 2$. Similarly, it is said to be an <strong> increasing sequence </strong> and denoted by $$A_n \uparrow A$$ if $A_j \subseteq A_{j+1}$ for each $j \ge 1$.

* Let $$\mathcal{G} \subseteq \mathcal{P}(X)$$ be a collections of sets. Then $$\mathcal{G}$$ is called a <strong> monotone class </strong>if it is closed for every increasing or decreasing sequence of sets. Meaning that If $$A_n \uparrow A$$ or $$A_n \downarrow A$$ then $A \in \mathcal{G}$. Notice that the intersection of any arbitrary collection of monotone classes is a monotone class. Hence, we can think of the notion that of a monotone class being generated by any collection of sets. 

* We say that a set function $\mu$ is <strong>continuous from above</strong> if it converges for all decreasing sequences i.e. $\mu \left( \bigcap_{j=1}^{\infty}A_j \right) = \lim_{n \to \infty} \mu(A_n)$. We say that $\mu$ is <strong>continuous from below</strong> if it converges for all increasing sequences i.e. $\mu \left( \bigcup_{j=1}^{\infty}A_j \right) = \lim_{n \to \infty} \mu(A_n)$. We say that it is <strong>continuous</strong> if it is both continuous from below and above. 

* The collection $$\mathcal{B}(X)$$ of <strong>Borel sets</strong> in $X \subseteq \mathbb{R}$ is the smallest $\sigma$-algebra in $\mathcal{P}(X)$ containing the open sets in $X$. If $X$ is a topological space, then $$\mathcal{B} = \mathcal{B}(X)$$ is called the <strong>Borel algebra</strong>.

* Let $$\mathcal{A} \subseteq \mathcal{P}(X)$$ and let $\mu: \mathcal{A} \to \mathbb{R}^{+}\cup \{\infty\}$ be a set function. Then $\mu$ is <strong> finitely additive</strong> on $\mathcal{A}$ if for each finite sequence $\{A_n: n=1,\dots, N\} \subseteq \mathcal{A}$ of mutually disjoint sets we have

$$\mu \left( \bigcup_{n=1}^N A_n \right)= \sum_{n=1}^N \mu(A_n)$$

* $\mu$ defined above is <strong>$\sigma$-additive</strong> or <strong>countably additive</strong> on $\mathcal{A}$ if for each sequence $\{A_n: n=1,\dots\} \subseteq \mathcal{A}$ of mutually disjoint sets we have

$$\mu \left( \bigcup_{n=1}^{\infty} A_n \right)= \sum_{n=1}^{\infty} \mu(A_n)$$

* A nonnegative, $\sigma$-additive set function $\mu: \mathcal{A} \to \mathbb{R}^{+}\cup \{\infty\}$ is called a <strong>measure</strong>.

* For any interval $I \subseteq \mathbb{R}$, the <strong>Lebesgue measure</strong> $m(I)$ is the length of $I$. For any set $A \subseteq \mathbb{R}$, the <strong>Lebesgue outer measure</strong> of $A$ is defined as 

$$m^{*}(A) = \inf \left\{ \sum_{n=1}^{\infty} m(I_n) : A \subseteq \bigcup_{n=1}^{\infty} I_n ~ and~ \{I_n\} ~ is ~ a ~ countable ~ family ~ of ~ open ~ intervals \right\}$$

* <strong>Caratheodory approach: </strong> A set $A \subseteq \mathbb{R}$ is <strong>Lebesgue measurable</strong> if $\forall E \subseteq \mathbb{R}$, we have $$m^{*}(E) = m^{*}(E \cap A)+ m^{*}(E \cup A^c)$$ i.e. no matter how you cut it (with any $E$), $$m^{*}$$ is nicely additive. 

* If a set $A$ is Lebesgue measurable as defined above, then its <strong>Lebesgue measure</strong> is defined as $$m(A) = m^{*}(A)$$.

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space and let $S(x)$ be a statement about a point $x \in X$. We say that $S(x)$ is valid <strong> almost everywhere</strong> if there is a set $$N \in \mathcal{A}$$ for which $\mu(N) = 0$ such that $S(x)$ is true for $\forall x \in X \setminus N$. In this case, we write <strong> $S$ $\mu$-a.e.</strong>

* Let $f, f_n : X \to \mathbb{R}$ be functions on $X \subseteq \mathbb{R}$. The sequence $f_n$ <strong> converges pointwise</strong> to $f$ if $\forall x \in X, \forall \epsilon >0, \exists N > 0$ such that $\forall n \ge N$, we have $\lvert f_n(x) - f(x) \rvert < \epsilon$. We say that $f_n$ <strong> converges uniformly</strong> on $X$ to $f$ if $\forall \epsilon > 0, \exists N > 0$ such that $\forall n \ge N$ and $\forall x \in X$ we have $\lvert f_n(x) - f(x) \rvert < \epsilon$. 

* Let $$(X, \mathcal{A})$$ and $$(Y, \mathcal{E})$$ be measurable spaces. A function $f: X \to Y$ is <strong>measurable</strong> if $$\forall E \in \mathcal{E}$$ we have $$f^{-1}(E) \in \mathcal{A}$$.

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space and let $1 \le p < \infty$. We define <strong>$$\mathcal{L}_{\mu}^p(X)$$</strong> as the set of all $\mu$-measurable functions $$f:X \to \mathbb{C}$$ such that 

$$\int_{X} |f|^p \mathrm{d}\mu <\infty$$

* We write $f \sim g$ for $f, g \in \mathcal{L}_{\mu}^p(X)$ if $f=g$, $\mu$-a.e. and we note that $\sim$ is an equivalence relation. We define the space <strong>$${L}_{\mu}^p(X)$$</strong> to be the collection of all equivalence classes in $$\mathcal{L}_{\mu}^p(X)$$. Moreover, we set 

$${\lVert f \rVert}_p = \left(\int_{X} |g|^p \mathrm{d}\mu \right)^{\frac{1}{p}}$$

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space and let $f : X \to \mathbb{C}$ be measurable. We define  

$$ \mathrm{ess sup}_{x \in X} \lvert f(x) \rvert = {\lVert f \rVert}_{\infty} = \inf \{ M : \mu \left( \{ x : \lvert f(x) \rvert > M \}\right) = 0\}$$

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space and let $f, g : X \to \mathbb{C}$ be measurable functions. We can prove that $$d(f,g) =  \mathrm{ess sup}_{x \in X} \lvert f(x) - g(x) \rvert$$ is a distance. 

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space and $f_n, f : X \to \mathbb{R} \cup \\{\infty\\}$. Then we say that $f_n \to f$ <strong>almost uniformly</strong> if  $\forall \epsilon >0, \exists E \in \mathcal{A}$ such that $\mu(E^c)<\epsilon$ and $f_n \to f$ uniformly on $E$. It can be easily proved that if $f_n \to f$ almost uniformly, then $f_n \to f$ uniformly $\mu$-a.e. <strong>Egorov Theorem</strong>  shows that if $f_n \to f$ uniformly $\mu$-a.e. then $f_n \to f$ almost uniformly. Notice that the condition that $f_n \to f$ uniformly $\mu$-a.e. is stronger than both of these concepts and in that case both $f_n \to f$ almost uniformly and $f_n \to f$ $\mu$-a.e. 

* <strong>$$\mathcal{L}_{\mu}^{\infty}(X)$$</strong> is the set of all measurable functions $f : X \to \mathbb{C}$ such that $$ \lVert f \rVert_{\infty}< \infty$$. Also <strong>$${L}_{\mu}^{\infty}(X)$$</strong>  is the set of equivalence classes defined by the equivalence relation $$\sim$$ on <strong>$$\mathcal{L}_{\mu}^{\infty}(X)$$</strong>. 

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space. We say that $\\{A_n : n=1,\dots, N\\} is a finite collection of <strong>independent sets</strong> if 
 $$\mu\left(A_{n_1}\cap \dots A_{n_k} \right)= \mu\left(A_{n_1}\right) \dots \mu \left(A_{n_k}\right)$$ for all $k \le N$ and $n_1 < \dots < n_k \le N$. An infinite collection of measurable sets is <strong>independent</strong> of each other if each of its finite subcollections is independent. 

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space. We say that $$(X, \mathcal{A}, \mu)$$ or $X$ is a <strong>finite measure space</strong> and $\mu$ is a <strong> bounded measure</strong> if $\mu(X) < \infty$. We say that $X$ or $\mu$ is <strong>$\sigma$-finite</strong> if $$\exists \{A_n : n=1,\dots\} \subseteq \mathcal{A}$$ such that $\forall n, \mu(A_n) < \infty$ and $$\bigcup_{n=1}^{\infty} A_n = X$$. Clearly, we can always take $$\{A_n : n=1,\dots\}$$ to be a disjoint family. 

* A measure space $$(X, \mathcal{A}, \mu)$$ is <strong>complete</strong> if $$\forall A \in \mathcal{A}$$ for which $$\mu(A)=0$$, and $$\forall B \subseteq A$$, we have $$B \in \mathcal{A}$$ and thus $\mu(B) = 0$. For instance, $$(\mathbb{R}, \mathcal{M}(\mathbb{R}), m)$$ is complete while $$(\mathbb{R}, \mathcal{B}(\mathbb{R}), m)$$ is not complete. 

* An <strong>outer measure</strong> is a nonnegative set function $$\mu^{*} : \mathcal{P}(X) \to \mathbb{R}^{*}$$ such that 
	- If $$A \subseteq B$$, then $$\mu^{*}(A) \le \mu^{*}(B)$$.
	- $\mu^{*}(\emptyset)=0$.
	- $$\mu^{*}(\bigcup_{n=1}^{\infty}) \le \sum_{n=1}^{\infty}\mu^{*}(A_n)$$.

&nbsp; &nbsp; &nbsp; Notice that outer measure does not require $\sigma$-addititivity. It only requires $\sigma$-subadditivity. 

* The outer measure $\mu^{*}$ can be constructed from measure $\mu$ as $$\mu^{*}(E) = \inf \left\{\sum_{j=1}^{\infty} \mu(A_j) \right\}$$ for $\forall E \in \mathcal{P}(X)$ where the infimum is taken over all the collections $$\{A_j : j=1,\dots\} \subseteq \mathcal{A}$$ that cover $E$.

## Theorems 

* If $X$ is a Hausdorff space, then a sequence of points $X$ converges to at most one point of $X$. 

* Every closed subspace of a compact space is compact. 

* Every compact subspace of a Hausdorff space is closed. 

* The image of a compact space under a continuous map is compact. 

* Let $f: X \to Y$ be a bijective continuous function. If $X$ is compact and $Y$ is Hausdorff then $f$ is a homeomorphism. 

* Let $X$ be a simply ordered set having the least upper bound property. In the order topology, each closed interval in $X$ is compact. This proves that every closed interval in $\mathbb{R}$ is compact. 

* <strong>Extreme Value Theorem:</strong> Let $f :X \to Y$ be continuous, where $Y$ is an ordered set in the order topology. If $X$ is compact, then there exist points $c$ and $d$ in $X$ such that $f(c) \le f(x) \le f(d)$ for every $x \in X$.

* Let $f:X \to Y$ be a continuous map of the compact metric space $(X, d_x)$ to the metric space $(Y,d_y)$. Then $f$ is uniformly continuous. 

* Let $X$ be a nonempty compact Hausdorff space. If $X$ has no isolated points, then $X$ is uncountable. 

* Compactness implies limit point compactness but not conversely. 

* Let $X$ be a metrizable space. Then the following are equivalent. 
	- $X$ is compact. 
	- $X$ is limit point compact.
	- $X$ is sequentially compact. 

* Every simply ordered set $X$ having the least upper bound property is locally compact. 

* Let $X$ be a Hausdorff space. Then $X$ is locall compact if and only if given $x \in X$ and given a neighborhood $U$ of $x$, there is a neighborhood $V$ of $x$ such that $\bar{V}$ is compact and $\bar{V} \subseteq U$. 

* Let $X$ be a locally compact Hausdorff space and let $A$ be a subspace of $X$. If $A$ is closed in $X$ or open in $X$, then $A$ is locally compact.

* Metric spaces are Hausdorff. 

* <strong> Urysohn Lemma: </strong> Let $X$ be a locally compact Hausdorff space. If $K \subseteq X$ is compact and $U \subseteq X$ is an open set containing $K$, then there is a continuous function $f: X \to [0,1]$ such that $f=1$ on $K$ and $f=0$ on $U^c$.

* Let $(X, \rho)$ be a metric space. A subset $K \subseteq X$ is compact if and only if every sequence has a convergent subsequence. 

* A normed vector space $X$ is a Banach space if and only if every absolutely convergent series is convergent.

* <strong>Cauchy Criterion: </strong>Let $$\{f_n : n = 1, \dots\}$$ be a sequence of real-valued functions on $$X \subseteq \mathbb{R}$$. Then $$\{f_n\}$$ converges uniformly on $X$ (to some function $f$) if and only if $\forall \epsilon >0, \exists N > 0$ such that $\forall m,n > N$ and $\forall x \in X$ we have $$\lvert f_m(x) - f_n(x) \rvert < \epsilon$$.

* Any intersection of algebras in an algebra. Any intersection of $\sigma$-algebras is a $\sigma$-algebra. 

* Let $\mathcal{S} \in \mathcal{P}(X)$ be a semialgebra and let $\mathcal{Q}(\mathcal{S})$ be the algebra generated by $\mathcal{S}$. If $A \in \mathcal{Q}(\mathcal{S})$, then it can be written as a finite disjoint union of the elements of the semialgebra, i.e. $\exists E_j \in \mathcal{S}$ such that $A = \bigcup_{j=1}^n E_j$.

* Let $\mathcal{A} \in \mathcal{P}(X)$ be an algebra and assume that $\mu : \mathcal{A} \to \mathbb{R}^{+} \bigcup \\{\infty\\}$ be additive on $\mathcal{A}$. 
	- If $\mu$ is $\sigma$-additive, then $\mu$ is continuous on $E$,   $\forall E \in \mathcal{A}$.
	- If $\mu$ is continuous from below on all $\forall E \in \mathcal{A}$ then $\mu$ is $\sigma$-additive.
	- If $\mu$ is continuous from above on the empty set $\emptyset$ and $\mu$ is finite, then $\mu$ is $\sigma$-additive.

* Let $\mathcal{S} \in \mathcal{P}(X)$ be a semialgebra and assume that $\mu : \mathcal{A} \to \mathbb{R}^{+} \bigcup \\{\infty\\}$ is additive (respectively, $\sigma$-additive). We can extend this function $\mu$ on the algebra $\mathcal{Q}(\mathcal{S})$ generated by this semialgebra as follows: 
	- $$\exists \nu : \mathcal{Q}(\mathcal{S}) \to \mathbb{R}^{+} \bigcup \{\infty\}$$ such that $\nu$ is additive (respectively, $\sigma$-additive). 
	- For any $A \in \mathcal{S}$ we have $\mu(A) = \nu(A)$.
	- $\nu$ is unique. Meaning that if we have $\mu_1, \mu_2$ defined on $$ \mathcal{Q}(\mathcal{S})$$, and if we have $\mu_1(A) = \mu_2(A)$ for $\forall A \in \mathcal{S}$, then $\mu_1(E) = \mu_2(E)$ for $\forall E \in  \mathcal{Q}(\mathcal{S})$. 
 
 * As we saw above, a set function on a semialgebra can be extended to a set function on an algebra. We can further extend it in a unique way to a measure on the $\sigma$-algebra generated by the algebra. To do so, we define a function $$\pi^{*}: \mathcal{P}(X) \to \mathbb{R}^{*}$$ on all subsets of $X$ as follows. For $\forall A \in X$ we define,

$$\pi^{*}(A) \triangleq \inf_{E_j} \left\{ \sum_{j=1}^{\infty} \nu(E_j), ~ for ~  E_j \in \mathcal{Q}(\mathcal{S}) ~ such ~ that ~ A \in \bigcup_{j=1}^{\infty}E_j \right\}$$

&nbsp; &nbsp; &nbsp; This is a well defined measure and it can be proved that it is an outer measure. Next, we define the 
&nbsp; &nbsp; &nbsp; &nbsp;  set $\mathcal{M}$ of all measurable subsets of $\mathcal{P}(X)$ as the collection of sets that can correctly divide all the
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; sets. In other words, $A \in \mathcal{M}$ if $\forall E \in X$ we have $$\pi^{*}(A) = \pi^{*}(A \cup E) + \pi^{*}(A \cap E)$$. It can be
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; proved that $$\mathcal{M}$$ is a $\sigma$-algebra and $$\mathcal{Q}(\mathcal{S}) \in \mathcal{M}$$. Hence, $$\mathcal{M}$$ contains the $\sigma$-algebra generated by 
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  the algebra $$\mathcal{Q}(\mathcal{S})$$. Notice that $\pi^{*}$ is defined for all subsets, but if we consider its restriction over
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  $\mathcal{M}$, it can be proved that it will be $\sigma$-additive, further, it can be proved that for $\forall A \in \mathcal{Q}(\mathcal{S})$ we have
&nbsp; &nbsp; &nbsp; &nbsp; $$\pi^{*}(A) = \nu(A)$$. This allows us to introduce an extension to the $\sigma$-algebra which can be proved to
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  be unique if $\mu$ is $\sigma$-finite.

* The monotone class generated by an algebra is equal to the $\sigma$-algebra generated by the same algebra. 

* If $$\mathcal{B}(\mathbb{R})$$ is the set of all Borel sets in $$\mathbb{R}$$ and $$\mathcal{M}(\mathbb{R})$$ is the set of all Lebesgue measurable  subsets of $$\mathbb{R}$$, we can prove that
	- $$\mathcal{M}(\mathbb{R})$$ is a $\sigma$-algebra.
	- We have $$\mathcal{B}(\mathbb{R}) \subseteq \mathcal{M}(\mathbb{R})$$ 
	- $$\mathcal{M}(\mathbb{R}) \setminus \mathcal{B}(\mathbb{R}) \neq \emptyset$$. Hence there are measurable sets that are not Borel.  
	- If $$A \in \mathcal{M}(\mathbb{R})$$, then $$\exists B \in \mathcal{B}(\mathbb{R})$$ and $$\exists E \in \mathcal{M}(\mathbb{R})$$ with $m(E)=0$ such that $A= B \cup E$ and $B \cap E = \emptyset$ and $m(A)=m(B)$. i.e. every Lebesgue measurable set is a Borel set up to a set of measure zero. 
	- There exists sets that are not Lebesgue measurable and it is very difficult to approximate these nonmeasurable sets with measurable ones. More formally, let $$E \subseteq \mathbb{R}$$ be nonmeasurable. There is $\epsilon > 0$ such that if $E \subseteq A$ and $E^c \subseteq B$ where $A$ and $B$ are measurable, then $m(A \cap B) > \epsilon$.

* Let $$(X, \mathcal{A}, \mu)$$ be a measure space. 
	- If $A,B \in \mathcal{A}, A \subseteq B$, then $\mu(A) \le \mu(B)$.
	- For each sequence $\\{A_n : n=1,\dots\\} \subseteq \mathcal{A}$ we have $$\mu \left( \bigcup_{n=1}^{\infty} A_n\right) \le \sum_{n=1}^{\infty} \mu(A_n)$$.
	- If $\\{A_n : n=1,\dots\\} \subseteq \mathcal{A}$ statisfies the condition that $\mu(A_1) < \infty$ and $A_n \subseteq A_{n-1}$ for each $n \ge 2$, then $$\mu \left( \bigcap_{n=1}^{\infty} A_n\right) = \lim_{n \to \infty} \mu(A_n)$$. 
	- If $\\{A_n : n=1,\dots\\} \subseteq \mathcal{A}$ is a sequence with the property that $A_n \subseteq A_{n+1}$ for each $n \ge 1$, then $$\mu \left( \bigcup_{n=1}^{\infty} A_n\right) = \lim_{n \to \infty} \mu(A_n)$$. 

* <strong>First Borel-Cantelli lemma:</strong> Let $$(X, \mathcal{A}, \mu)$$ be a measure space. If $\\{A_n : n=1,\dots\\} \subseteq \mathcal{A}$ statisfies the condition that $\sum_{n=1}^{\infty } \mu(A_n) < \infty$, then the collection of all those $x \in X$ that belong to infinitely many sets $A_n$ has measure 0. Formally, 

$$\mu \left( \bigcap_{m=1}^{\infty} \bigcup_{n=m}^{\infty} \right) = 0$$

* <strong>Second Borel-Cantelli lemma:</strong> Let $$(X, \mathcal{A}, \mu)$$ be a measure space such that $\mu(X) = 1$. If $\\{A_n : n=1,\dots\\} \subseteq \mathcal{A}$ is a sequence of independent sets and $\sum_{n=1}^{\infty } \mu(A_n) = \infty$, then 

$$\mu \left( \bigcap_{m=1}^{\infty} \bigcup_{n=m}^{\infty} \right) = 1$$

* <strong>Kolmogorov zero-one law:</strong>  Let $$(X, \mathcal{A}, \mu)$$ be a measure space and let $\\{A_n : n=1,\dots\\} \subseteq \mathcal{A}$ be a sequence of independent sets. Let $$\mathcal{A}_m$$ denote the $\sigma$-algebra generated by $\\{A_n : n=m,\dots\\}$. 	For each $A \in \cap_{m=1}^{\infty}\mathcal{A}_m$, either $\mu(A)=0$ or $\mu(A)=1$. 

* <strong>Measure Completeness Theorem</strong> Let $$(X, \mathcal{A}, \mu)$$ be a measure space. There is a measure space Let $$(X, \mathcal{A}_0, \mu_0)$$, called the complete measure space corresponding to $$(X, \mathcal{A}, \mu)$$ such that
	- $$\mathcal{A} \subseteq \mathcal{A}_0$$.
	- $\mu = \mu_0$ on $\mathcal{A}$.
	- $$A \in \mathcal{A}_0 \Longleftrightarrow A = B \cup E $$ where $B \in \mathcal{A}$ and $E \subseteq{D}$ for some $D \in \mathcal{A}$ that satisfies $\mu(D) = 0$.
	- If $A \in \mathcal{A}_0, \mu_0(A) = 0$ and $S \subseteq A$, then  $S \subseteq \mathcal{A}_0$ and $\mu_0(S)=0$.
	
* Let $$(X, \mathcal{A}, \mu)$$ be a complete measure space. If $f$ is measurable and $f=g$, $\mu$-a.e. then $g$ is measurable. This allows us to define an equivalence class $\sim$ where $f \sim g$ if $f=g$, $\mu$-a.e. 

* If $f \sim g$ then we have $$\mathrm{ess sup}\lvert f \rvert = \mathrm{ess sup}\lvert g \rvert$$.

* Let $f_n, f : X \to \mathbb{R} \cup \\{\infty\\}$. Then $$f_n \to f$$ uniformly $\mu$-a.e. $\Longleftrightarrow d(f_n, f) = \mathrm{ess sup}\lvert f_n - f \rvert \to 0$. 

* If $X$ is a topological space and $$(X, \mathcal{A}, \mu)$$ is a measure space, assuming that $\mathcal{B}(X) \subseteq \mathcal{A}$, every continuous function $f : X \to \mathbb{R}$ is $\mathcal{B}(X)$ measurable. 

* There exists a perfect symmetric set having positive Lebesgue measure with a subset that is not Lebesgue measurable. 