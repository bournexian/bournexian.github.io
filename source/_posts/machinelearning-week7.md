---
title: machinelearning-week7
date: 2017-10-04 16:27:15
tags: machinelearning
mathjax: true
---

## 机器学习 第7周

第七周，主要是监督学习方面的SVM算法, 区别于之前几周学的Logistic Regression.


### Formula
 {% raw %}
$$J(\theta) = \frac{1}{m} \sum_{i=1}^m y^{(i)} \ \text{cost}_1(\theta^Tx^{(i)}) + (1 - y^{(i)}) \ \text{cost}_0(\theta^Tx^{(i)}) + \dfrac{\lambda}{2m}\sum_{j=1}^n \Theta^2_j$$
 {% endraw %}

这里可以看作是找出一个向量，使得不同样本的投影到这个向量的距离最大化。

### Kernels
Gaussian Kernel:
 {% raw %}
$$f_i = similarity(x, l^{(i)}) = \exp(-\dfrac{\sum^n_{j=1}(x_j-l_j^{(i)})^2}{2\sigma^2})$$
 {% endraw %}

还未深入了解，要先补补数学基础，不过这个Kernel函数应该是可以简化SVM的计算，所以常常一起配合使用。


### Logistic Regression vs. SVMs

If n is large (relative to m), then use logistic regression, or SVM without a kernel (the "linear kernel")

If n is small and m is intermediate, then use SVM with a Gaussian Kernel

If n is small and m is large, then manually create/add more features, then use logistic regression or SVM without a kernel.