---
title: machinelearning-week2
date: 2017-08-15 20:43:22
tags: machinelearning
mathjax: true
---

## 机器学习 第二周
下面是一些讲义的摘抄，以及一些学习笔记。
MathJax写公式上手还是比较容易的。

### Linear regression

#### Hypothesis:

$h_\theta(x) = \theta^Tx = \theta_0x_0 + \theta_1x_1 + ··· + \theta_nx_n$

#### Parameters: 

$\theta_0,\theta_1...,\theta_n$


#### Cost function:
{% raw %}
$J(\theta) = \frac 1 {2m} \sum_{i=1}^m (h_\theta(x^{(i)}-y^{(i)})^2$
{% endraw %}
#### Gradient descent:
Repeat{
    $\theta_j := \theta_j - \alpha \frac \partial {\partial \theta_j}J(\theta)$
}

#### Feature Scaling:
To make sure features are on a similar scale, so that it could be faster when do the gradient descent.

#### Mean normalization:
To make features have approximately zero mean.

$x_i = \frac {x_i - \mu_i} {s_i}$

$\mu_i$ is the average value of x_i in traning set
$s_i$ is the range (max-min)  or (standard deviation)

#### Learning rate:
It's better to draw a graph for the value of $J(\theta)$. It it decrease after every iteration then it is good.
Otherwise, if it is slow convergence or value of cost function not decrease, consider to change the learning rate $\alpha$.

Reference: [https://www.coursera.org/learn/machine-learning](https://www.coursera.org/learn/machine-learning)