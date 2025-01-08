---
layout: post
title:  "Mantel Haenszel (MH) Test"
date:   2025-01-05 00:00:00 -0000
categories: machinelearning
---

In this page, we explain the details of Mantel Haenszel (MH) Test which is often used in statistical analysis to calculate an adjusted odds ratio or relative risk while accounting for stratification across different groups or strata. The MH method is primarily used when we want to compare two groups (e.g., treatment vs. control) across several strata (e.g., different age groups, locations, or study sites). The method ensures that differences between strata are accounted for, preventing bias from confounding factors and it adjusts for differences in group sizes and event rates by weighting each stratum's contribution proportionally. The formula for calculating the Mantel-Haenszel ratio is as following:

$$
\begin{align*} \mathrm{MH}(k_{a,i}, n_{a,i}, k_{b,i}, n_{b,i}) &= \frac{\sum_i \frac{k_{a,i}}{n_{a,i}} \cdot w_i} {\sum_i \frac{k_{b,i}}{n_{b,i}} \cdot w_i}, \text{where } w_i = \frac{n_{a,i} \cdot n_{b,i}} {n_{a,i} + n_{b,i}} \\ &= \frac{\sum_i r_{a,i} \cdot w_i} {\sum_i r_{b,i} \cdot w_i}, \text{where } r_{x,i} = \frac{k_{x,i}}{n_{x,i}} \end{align*}
$$

in which

* $k_{a,i}$ is the number of events (or successes) in group $a$ for stratum $i$. 
* $k_{b,i}$ is the number of events (or successes) in group $b$ for stratum $i$.
* $n_{a,i}$ is the total number of observations in group $a$ for stratum $i$.
* $n_{b,i}$ is the total number of observations in group $b$ for stratum $i$.
* $r_{x,i} = \frac{k_{x,i}}{n_{x,i}}$ is the proportion of events in group $x$ (either $a$ or $b$) for stratum $i$.
* $w_i = \frac{n_{a,i} n_{b,i}}{n_{a,i} + n_{b,i}}$ is the weight for stratum $i$, which is proportioal to the harmonic mean of the group sizes $n_{a,i}$ and $n_{b,i}$. 
