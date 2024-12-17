---
layout: post
title:  "Distillation"
date:   2025-01-02 00:00:00 -0000
categories: machinelearning
---

In this page, I will talk about few tips related to distillation in neural networks. Main reference for this page is the original [distillation paper](https://arxiv.org/pdf/1503.02531).

# Distillation

* When the soft targets have high entropy, they provide much more information per training case than hard targets and much less variance in the gradient between training cases, so the small model can often be trained on much
less data than the original cumbersome model and using a much higher learning rate.

* In distillation, the temperature of the final softmax is raised until the cumbersome model produces a suitably soft set of targets. We then use the same high temperature when training the small model to match these
soft targets.

* Let the logits for the student model prior to softmax be $z_i$. These logits are converted to probabilities $q_i$ using a temperature $T$ according to the following equation in which $T$ is a temperature that is normally set to 1. Using higher values for $T$ produces a softer probability distribution over classes with higher entropy.

$$
q_i = \frac{\exp(z_i/T)}{\sum_j \exp(z_j/T)}
$$

* In the simplest form of distillation, knowledge is transferred to the distilled model by training it on
a transfer set and using a soft target distribution for each case in the transfer set that is produced by
using the cumbersome model with a high temperature in its softmax. The same high temperature is
used when training the distilled model, but after it has been trained it uses a temperature of 1.

* When the correct labels are known for all or some of the transfer set, this method can be significantly
improved by also training the distilled model to produce the correct labels. One way to do this is
to use the correct labels to modify the soft targets, but a better way is to simply use
a weighted average of two different objective functions. 

* The first objective function is the cross entropy with the soft targets and this cross entropy is computed using the same high temperature in the softmax of the distilled model as was used for generating the soft targets from the cumbersome model. If the cumbersome model produces soft-target probabilies $p_i$, then the first objective function can be written as

$$
C_1 = - \sum_{i=1}^{N} p_i \log(q_i) 
$$

* The second objective function is the cross entropy with the correct labels. This is computed
using exactly the same logits in softmax of the distilled model but at a temperature of 1.

* The best results are generally obtained by using a condiderably lower weight on the second
objective function. Since the magnitudes of the gradients produced by the soft targets scale as $1/{T^2}$
it is important to multiply them by $T^2$ when using both hard and soft targets. This ensures that the
relative contributions of the hard and soft targets remain roughly unchanged if the temperature used
for distillation is changed while experimenting with meta-parameters.

* If the cumbersome model has logits $v_i$, then for high temperatures and for the case  that the logits have been zero-meaned separately for each transfer case ($\sum_j z_j = \sum_j v_j = 0$) we can show that 

$$
\frac{\partial C_1}{\partial z_i} = \frac{1}{T}(q_i - p_i) = \frac{1}{N T^2}(z_i - v_i) 
$$

* So in the high temperature limit, distillation is equivalent to minimizing $1/2(z_i − v_i)^2$, provided the
logits are zero-meaned separately for each transfer case. At lower temperatures, distillation pays much less attention to matching logits that are much more negative than the average. This is potentially advantageous because these logits are almost completely unconstrained by the cost function
used for training the cumbersome model so they could be very noisy. 

* On the other hand, the very negative logits may convey useful information about the knowledge acquired by the cumbersome
model. Which of these effects dominates is an empirical question. When the distilled
model is much too small to capture all of the knowledege in the cumbersome model, intermediate temperatures work best which strongly suggests that ignoring the large negative logits can be
helpful.
