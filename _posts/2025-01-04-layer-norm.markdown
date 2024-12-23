---
layout: post
title:  "Layer Normalization"
date:   2025-01-04 00:00:00 -0000
categories: machinelearning
---

In this page, I will talk about few tips related to layer normalization in neural networks. Main reference for this page is the original [batch normalization paper](https://arxiv.org/pdf/1607.06450).

# Layer normalization

* Batch normalization cannot be obviously applied to RNNs due to the varying length of the inputs and the difficulty of keeping running averages of the input stream. Further, the effect of batch normalization depends on the batch size.
Furthermore, batch normalization cannot be applied to online learning tasks or to extremely large distributed models where the minibatches have to be small. Layer normalization is designed to overcoem the shortcomings of batch normalization.

* In layer normalization the mean and variance used for normalization are found by summing all the
inputs to the neurons in a layer on a \em{single training} case. Like batch normalization,
we also give each neuron its own adaptive bias and gain which are applied after
the normalization but before the non-linearity. Unlike batch normalization, layer
normalization performs exactly the same computation at training and test times.

* Changes in the output of one layer will tend to cause highly correlated changes in the
 inputs to the next layer, especially with ReLU units whose outputs can change by a lot.
This suggests the ``covariate shift'' problem can be reduced by fixing the mean and the variance of
the summed inputs within each layer. Layer normalization thus computes statistics over all
the hidden units in the same layer. All the hidden units in a layer share the same normalization terms
$\mu$ and $\sigma$, but different training cases have different normalization terms. Unlike batch normalization,
layer normaliztion does not impose any constraint on the size of a mini-batch and it can be used in
the pure online regime with batch size of 1. 

* The trainable scale and shift parameters are then computed similar to batch normalization. The key difference is that LayerNorm normalizes across features for each sample individually as opposed to BatchNorm which normalizes across a batch of samples. 

* LayerNorm is often preferred in sequence models (like transformers) or scenarios where batch sizes are small or vary.

* During testing (or inference), Layer Normalization (LayerNorm) works the same way as it does during training because it does not depend on the statistics of the entire dataset or mini-batches. It doesn't need to keep a running statistic of batches and the normalization remains specific to each input sample. The learnable scale and shift parameters are applied to the normalized activations just as in training. These parameters are fixed during testing because they were learned during training.

* LayerNorm's calculations depend only on the features of the current input and not on other samples in the batch or dataset. This makes its behavior during testing identical to training, ensuring consistency in the model's predictions.
