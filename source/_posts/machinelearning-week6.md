---
title: machinelearning-week6
date: 2017-09-18 22:57:47
tags: machinelearning
mathjax: true
---


## 机器学习 第6周

第六周主要讲的是如何评估当前的算法并进行改进. 下面是lecture notes的摘抄.

### Train/Validation/Test Sets
这里主要引入了一个cross validation的概念，比如把总体样本分成3份，60%作为training set， 20%作为Cross Validation，剩下的作为test set; 然后基于下面的方法来计算不同的偏差值:

1. Optimize the parameters in Θ using the training set for each polynomial degree.
2. Find the polynomial degree d with the least error using the **cross validation** set.
3. Estimate the generalization error using the test set with Jtest(Θ(d)), (d = theta from polynomial with lower error);

### Diagnosing Bias vs. Variance
* High bias (underfitting): both Jtrain(Θ) and JCV(Θ) will be high. Also, JCV(Θ)≈Jtrain(Θ).
* High variance (overfitting): Jtrain(Θ) will be low and JCV(Θ) will be much greater than Jtrain(Θ).

### Learning Curves
* If a learning algorithm is suffering from high bias, getting more training data will not (by itself) help much.
* If a learning algorithm is suffering from high variance, getting more training data is likely to help.

### Deside What to Do Next
* Getting more training examples: Fixes high variance
* Trying smaller sets of features: Fixes high variance
* Adding features: Fixes high bias
* Adding polynomial features: Fixes high bias
* Decreasing λ: Fixes high bias
* Increasing λ: Fixes high variance.