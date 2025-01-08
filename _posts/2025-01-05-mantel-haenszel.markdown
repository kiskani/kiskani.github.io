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
* $r_{x,i} = \frac{k_{x,i}}{n_{x,i}}$ is the proportion of events in group $x$ (either $a$ or $b$) for stratum $i$. Using these proportions standardizes the event counts relative to the group sizes.
* $w_i = \frac{n_{a,i} n_{b,i}}{n_{a,i} + n_{b,i}}$ is the weight for stratum $i$, which is proportioal to the harmonic mean of the group sizes $n_{a,i}$ and $n_{b,i}$. The weight $w_i$ for each stratum balances the contributions of the two groups. This accounts for the fact that strata with larger sample sizes provide more reliable estimates and should have greater influence on the combined estimate.

The numerator in MH statistic represents the adjusted, pooled event rate for group $a$, and the denominator represents the adjusted, pooled event rate for group $b$. By stratifying and using weights, the MH estimator reduces the influence of differences between strata (e.g., different age distributions or study conditions) that could confound the comparison of groups. Instead of treating the data as a single large sample (which would ignore stratum-specific differences), the MH method respects the stratified structure and ensures the pooled estimate reflects the true relationship across all groups.

Harmonic weight is used for multiple reasons. One reason is that it downweights strata with imbalanced group sizes ($n_{a,i} >> n_{b,i}$ or vice versa) and priortizes strata with balanced group sizes. This is because the harmonic mean is smaller than the arithmatic mean when $n_{a,i}$ and $n_{b,i}$ differ significantly. Another reason is that arithmatic mean ignores the variance caused by smaller groups and geometric mean is less sensitive to the imbalance in group sizes than the harmonic mean. Further it also optimizes statistical efficiency by minimizing variance.  
