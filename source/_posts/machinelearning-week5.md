---
title: machinelearning-week5
date: 2017-09-12 21:39:25
tags: machinelearning
mathjax: true
---

## 机器学习 第5周
---

这周没什么update的，好像重点就是讲了下Backpropagation.

### Cost function
* L = total number of layers in the network
* sl = number of units (not counting bias unit) in layer l
* K = number of output units/classes

Cost function could be represented as:
 {% raw %}
$$
J(\theta) = - \frac{1}{m} \sum_{i=1}^m [ y^{(i)}\ \log (h_\theta (x^{(i)})) + (1 - y^{(i)})\ \log (1 - h_\theta(x^{(i)}))] + \frac{\lambda}{2m}\sum_{j=1}^n \theta_j^2
$$
 {% endraw %}

### To calculate delta:
 {% raw %}
$$
\Delta^{(l)} := \Delta^{(l)} + \delta^{(l+1)}(a^{(l)})^T
$$
 {% endraw %}

### Steps for back propagation

1. Set $ a^{(1)} := x^{(t)} $
2. Perform forward propagation to compute $a^{(l)}$ for l=2,3,…,L
3. Using $ y^{(t)} $, compute $ \delta^{(L)} = a^{(L)} - y^{(t)} $
4. Compute $ \delta^{(L-1)}, \delta^{(L-2)},\dots,\delta^{(2)} $ using {% raw %} $ \delta^{(l)} = ((\Theta^{(l)})^T \delta^{(l+1)})\ .*\ a^{(l)}\ .*\ (1 - a^{(l)})  {% endraw %} $
5. The capital-delta matrix D is used as an "accumulator" to add up our values as we go along and eventually compute our partial derivative.
{% raw %}
$$ 
\frac \partial {\partial \Theta_{ij}^{(l)}} J(\Theta) = D_{ij}^{(l)}
$$
 {% endraw %}
 and 
{% raw %}
$$
  D^{(l)}_{i,j} := \dfrac{1}{m}\left(\Delta^{(l)}_{i,j} + \lambda\Theta^{(l)}_{i,j}\right) 
$$
$$
  D^{(l)}_{i,j} := \dfrac{1}{m}\Delta^{(l)}_{i,j}
$$
 {% endraw %}


 Notes from:
 [coursera machine learning](https://www.coursera.org/learn/machine-learning/supplement/pjdBA/backpropagation-algorithm)