---
title: machinelearning-week3
date: 2017-08-27 11:23:25
tags: machinelearning
mathjax: true
---

## 机器学习 第三周
### Classification and Representation

Summary of lecture notes from my point of view...

#### Classification
* Binary classification problem
* Multi-class classification (one-vs-all)

### Logistic Regression
#### Hypothesis
* $ h_\theta(x)=g(\theta^Tx)  $
* $ z = \theta^Tx $ 
* $ g(z) = \frac 1 {1+e^(-z)}$
* *probability* $h_\theta(x)  = P(y=1|x;\theta) = 1-P(y=0|x;\theta)$


#### Cost function of logistic regression
A vectorized implementation:
$ h = g(X\theta) $
$J(\theta) = \frac 1 m * (-y^Tlog(h)-(1-y)^Tlog(1-h)) $

#### Gradient Descent
The algorithm is same as the one used in linear regression, only the meaning of h(\theta) is changed to sigmoid function.

Repeat{
    $\theta_j := \theta_j - \alpha \frac \partial {\partial \theta_j}J(\theta)$
}


### Solving the problem of overfitting
#### Overfitting
Overifitting is caused by a hypothesis function that fits the available data but does not generalize well to predict new data. It is usually caused by a complicated function that creates a lot of unnecessary curves and angles unrelated to the data.

#### Solution
1) Reduce the number of features:
* Manually select which features to keep.
* Use a model selection algorithm (studied later in the course).

2) Regularization
* Keep all the features, but reduce the magnitude of parameters θj.
* Regularization works well when we have a lot of slightly useful features.

#### Regularization
* Regularized cost function
>   {% raw %}
    $ min_\theta \frac 1 {2m} \sum_{i=1}^m (h_\theta(x^{(i)})-y^{(i)})^2 + \lambda \sum_{j=1}^n \theta_j^2 $ 
    {% endraw %}

* Regularized linear regression

 >  Repeat  {
    {% raw %}
    $\theta_0 := \theta_0 - \alpha \frac 1 m \sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_0^{(i)}$
    </br>
    $\theta_j := \theta_j - \alpha [(\frac 1 m \sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)}) + \frac \lambda m \theta_j] $
    {% endraw %}
    }

* Regularized normal equation
> $ \theta = (X^TX+\lambda * L)^{-1}X^Ty $
L is a matrix with 0 at the top left and 1's down the diagonal, with 0's everywhere else.

* Regularized logistic regression
>   {% raw %}
$ J(\theta) = - \frac 1 m \sum_{i=1}^m[y^{(i)}log(h_\theta(x^{(i)}))+(1-y^{(i)})log(1-h_\theta(x^{(i)}))] + \frac \lambda {2m} \sum_{j=1}^n\theta_j^2$
    {% endraw %}
