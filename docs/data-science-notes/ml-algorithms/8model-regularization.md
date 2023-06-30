---
layout: default
title: Model regularization
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/8model-regularization/
nav_order: 8
---

# Model regularization

Ways of constraining a model: 

* Constrain the number of degrees
* Constrain the weights (ridge, lasso, elastic net)
* Constrain the trees (minimum impurity decrease, number of sample per leaf, number of leaves, depth of the tree, etc.)
* Early stopping: First the validation error goes down but after a certain number of epochs it goes up again. Early stopping stops training as soon as validation error goes up again (or say after two trainings where it goes up).

## Ridge regression

Principle: Add $$\alpha \dfrac{1}{2}\sum\limits_{i=1}^{n}{\theta_i}^2$$ to the cost function so that the model is encouraged to (or deterred from) keep the model weights as small as possible (otherwise the cost increases):

$$
COST(\boldsymbol{\theta}) = \text{MSE}(\boldsymbol{\theta}) + \alpha \dfrac{1}{2}\sum\limits_{i=1}^{n}{\theta_i}^2
$$

Note:

* the regularization term should only be added to the cost function during training. Once the model is trained, you want to evaluate the model’s performance using the unregularized performance measure.
* the bias term is not regularised
* it is important to scale the data before performing ridge regression as it is sensitive of the scale of the input features
* the ridge penalty is also called the $$\ell_2$$ penalty as it is half the square of the  $$\ell_2$$ norm of the weight vector

### Closed form regression

```python
from sklearn.linear_model import Ridge
ridge_reg = Ridge(alpha=1, solver="cholesky", random_state=42)
ridge_reg.fit(X, y)
ridge_reg.predict([[1.5]])
```

### Stochastic gradient descent

```python
sgd_reg = SGDRegressor(max_iter=50, tol=-np.infty, penalty="l2", random_state=42)
sgd_reg.fit(X, y.ravel())
sgd_reg.predict([[1.5]])
```

## Lasso regression

Principle: Add $$\alpha \sum\limits_{i=1}^{n}\left| \theta_i \right|$$ to the cost function so that the model is encouraged to (or deterred from) keep the model weights as small as possible (otherwise the cost increases):

$$
COST(\boldsymbol{\theta}) = \text{MSE}(\boldsymbol{\theta}) + \alpha \sum\limits_{i=1}^{n}\left| \theta_i \right|
$$

Note:

- the regularization term should only be added to the cost function during training. Once the model is trained, you want to evaluate the model’s performance using the unregularized performance measure.
- the bias term is not regularised
- it is important to scale the data before performing ridge regression as it is sensitive of the scale of the input features
- the lasso penalty is also called the $$\ell_1$$ penalty as it is uses the $$\ell_1$$ norm of the weight vector
- lasso regularization tends to completely eliminate the weights of the least important features, therefore also useful for **feature selection**
- lasso stands for Least Absolute Shrinkage and Selection Operator

### Closed form regression

```python
from sklearn.linear_model import Lasso
lasso_reg = Lasso(alpha=0.1)
lasso_reg.fit(X, y)
lasso_reg.predict([[1.5]])
```

### Stochastic gradient descent

```python
sgd_reg = SGDRegressor(max_iter=50, tol=-np.infty, penalty="l1", random_state=42)
sgd_reg.fit(X, y.ravel())
sgd_reg.predict([[1.5]])
```

## Elastic Net

Elastic Net is a middle ground between Ridge Regression and Lasso Regression. When r = 0, Elastic Net is equivalent to Ridge Regression, and when r = 1, it is equivalent to Lasso Regression.

$$
J(\boldsymbol{\theta}) = \text{MSE}(\boldsymbol{\theta}) + r \alpha \sum\limits_{i=1}^{n}\left| \theta_i \right| + \dfrac{1 - r}{2} \alpha \sum\limits_{i=1}^{n}{{\theta_i}^2}
$$

```python
from sklearn.linear_model import ElasticNet
elastic_net = ElasticNet(alpha=0.1, l1_ratio=0.5, random_state=42)
elastic_net.fit(X, y)
elastic_net.predict([[1.5]])
```

## Which one to use?

**Ridge is a good default**, but if you suspect that only a few features are actually useful, you should prefer Lasso or Elastic Net since they tend to reduce the useless features’ weights down to zero as we have discussed. **In general, Elastic Net is preferred over Lasso** since Lasso may behave erratically when the number of features is greater than the number of training instances or when several features are strongly correlated.
