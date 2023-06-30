---
layout: default
title: Logistic and softmax regression
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/3logistic-and-softmax-regression/
nav_order: 3
---

# Logistic and softmax regression

## Logistic regression

### General

Just like a linear regression model, a logistic regression model computes a weighted sum of the input features (plus a bias term), but instead of outputting the result directly like the linear regression model does, it outputs the **logistic** of this result.

The logistic function is a sigmoid function (i.e. S-shaped) that outputs a number between 0 and 1: 

$$\sigma(t) = \dfrac{1}{1 + \exp(-t)}$$ 

The probability of pertaining to the positive class is  calculated as  $$\hat{p} = \sigma(\boldsymbol{\theta}^T \mathbf{x})$$

Classification is then performed as follows: $$\hat{y} =
\begin{cases}
  0 & \text{if } \hat{p} < 0.5 \\
  1 & \text{if } \hat{p} \geq 0.5
\end{cases}$$

### Loss function

The loss function for the logistic regression is the **log loss** function: 

$$
COST(\boldsymbol{\theta}) = -\dfrac{1}{m} \sum\limits_{i=1}^{m}{\left[ y^{(i)} log\left(\hat{p}^{(i)}\right) + (1 - y^{(i)}) log\left(1 - \hat{p}^{(i)}\right)\right]}
$$

This function makes sense as for:

* positive instances ($$y=1$$), $$-log(\hat{p})$$ grows quickly very large as the model predicts a probability close to zero (i.e. the model predicts with confidence that the instance belongs to the negative class). On the other hand, high probabilities are not penalized much and the long function is much less steep around 1.
* negative instances ($$y=0$$), $$-log(1-\hat{p})$$ grows quickly very large as the model predicts a probability close to 1 (i.e. the model predicts with confidence that the instance belongs to the positive class). On the other hand, low probabilities are not penalized much and the long function is much less steep around 1.

### Scikit-Learn

```python
from sklearn.linear_model import LogisticRegression
log_reg = LogisticRegression(solver="liblinear", random_state=42)
log_reg.fit(X, y)
```

## Softmax regression

### General

Generalisation of the logistic regression model to accomodate multiple classes (i.e. more than two). The softmax regression is also called the multinomial logistic regression.

The idea is quite simple: when given an instance $$\mathbf{x}$$, the Softmax Regression model first computes a score $$s_k(\mathbf{x})$$ for each class $$k$$, then estimates the probability of each class by applying the softmax function (also called the normalized exponential) to the scores.

$$
s_k(\mathbf{x}) = ({\boldsymbol{\theta}^{(k)}})^T \mathbf{x}
$$

Note that each class has its own dedicated parameter vector $${\boldsymbol{\theta}^{(k)}}$$. All these vectors are typically stored as rows in a parameter matrix $$\boldsymbol{\Theta}$$. 

Once you have computed the score of every class for the instance, you can estimate the probability that the instance belongs to class $$k$$ by running the scores through the softmax function: it computes the exponential of every score, then normalizes them (dividing by the sum of all the exponentials).

$$
\hat{p}_k = \sigma\left(\mathbf{s}(\mathbf{x})\right)_k = \dfrac{\exp\left(s_k(\mathbf{x})\right)}{\sum\limits_{j=1}^{K}{\exp\left(s_j(\mathbf{x})\right)}}
$$

Just like the logistic regression classifier, the softmax regression classifier predicts the class with the highest estimated probability (which is simply the class with the highest score):

$$
\hat{y} = \underset{k}{\operatorname{argmax}} \, \sigma\left(\mathbf{s}(\mathbf{x})\right)_k = \underset{k}{\operatorname{argmax}} \, s_k(\mathbf{x}) = \underset{k}{\operatorname{argmax}} \, \left( ({\boldsymbol{\theta}^{(k)}})^T \mathbf{x} \right)
$$

### Loss function

The loss function for the softmax regression is the **cross-entropy** cost function. It is essentially the log loss function extended for multiple classes.

$$
COST(\boldsymbol{\Theta}) = - \dfrac{1}{m}\sum\limits_{i=1}^{m}\sum\limits_{k=1}^{K}{y_k^{(i)}\log\left(\hat{p}_k^{(i)}\right)}
$$

### Scikit-Learn

```python
from sklearn.linear_model import LogisticRegression
softmax_reg = LogisticRegression(multi_class="multinomial",solver="lbfgs", C=10)
softmax_reg.fit(X, y)
```
