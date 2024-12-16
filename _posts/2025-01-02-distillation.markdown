---
layout: post
title:  "Tips on Distillation"
date:   2025-01-02 00:00:00 -0000
categories: machinelearning
---

In this page, I will talk about few tips related to distillation in neural networks. Main reference for this page is the original [distillation paper](https://arxiv.org/pdf/1503.02531).

# Distillation

* When the soft targets have high entropy, they provide much more information per training case than hard targets and much less variance in the gradient between training cases, so the small model can often be trained on much
less data than the original cumbersome model and using a much higher learning rate.

* In distillation, the temperature of the final softmax is raised until the cumbersome model produces a suitably soft set of targets. We then use the same high temperature when training the small model to match these
soft targets.

* In softmax, logits $z_i$ are converted to probabilities $q_i$ using a temperature $T$ according to the following equation.

$$
q_i = \frac{\exp(z_i/T)}{\sum_j \exp(z_j/T)}
$$

