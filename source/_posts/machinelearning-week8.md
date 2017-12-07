---
title: machinelearning-week8
date: 2017-10-04 16:30:19
tags:
mathjax: true
---

## 机器学习 第8周

这周开始介绍 非监督型学习， 区分于监督型学习 这里的training set不会带有标签（label）。

### K-Means

#### K-Means Algorithm
K-Means 是用来自动对数据分组最常见的算法，其步骤为：
1. Randomly initialize two points in the dataset called the cluster centroids.
2. Cluster assignment: assign all examples into one of two groups based on which cluster centroid the example is closest to.
3. Move centroid: compute the averages for all the points inside each of the two cluster centroid groups, then move the cluster centroid points to those averages.
4. Re-run (2) and (3) until we have found our clusters.

NOTE：
With k-means, it is not possible for the cost function to sometimes increase. It should always descend.

#### Random Initialization

1. Have K<m. That is, make sure the number of your clusters is less than the number of your training examples.
2. Randomly pick K training examples. (Not mentioned in the lecture, but also be sure the selected examples are unique).
3. Set μ1,…,μK equal to these K examples.

### Dimensionality Reduction

#### Data Compression & Visualization
Reduce the dimension of features if there are a lot of redundant data.

#### Principal Component Analysis Problem Formulation
The most popular dimensionality reduction algorithm is Principal Component Analysis (PCA)
The **goal of PCA** is to **reduce** the average of all the distances of every feature to the projection line. This is the projection error.

#### PCA Algorithm

* Given training set: x(1),x(2),…,x(m)
* Preprocess (feature scaling/mean normalization):
 {% raw %}
$$\mu_j = \dfrac{1}{m}\sum^m_{i=1}x_j^{(i)}$$
 {% endraw %}
* Replace each $x_j^{(i)}$ with $x_j^{(i)} - \mu_j$
* If different features on different scales (e.g., $x_1$ = size of house, $x_2$ = number of bedrooms), scale features to have comparable range of values.

#### Reconstruction
Get approximate value of original data by compressed data.
 {% raw %}
$$z \in \mathbb{R} \rightarrow x \in \mathbb{R}^2$$
$$x_{approx}^{(1)} = U_{reduce} \cdot z^{(1)}$$
 {% endraw %}