---
layout: default
title: Linear regression
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/1linear-regression/
nav_order: 1
---

# Linear regression

## Linear models

$$
\hat{y} = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \dots + \theta_n x_n
$$

or in vectorized form: 

$$
\hat{y} = h_{\boldsymbol{\theta}}(\mathbf{x}) = \boldsymbol{\theta} \cdot \mathbf{x}
$$

where: 
* $$\hat{y}$$ is the predicted value
* $$n$$ is the number of features
* $$x_i$$ is the the ith feature value
* $$\theta_i$$ is the ith parameter value


## Linear model to estimate

Throughout this page, we will use different methods for estimating the parameters of the linear model:

$$y = 4 + 3x$$

```python
import numpy as np
X = 2 * np.random.rand(100, 1) 
y = 4 + 3 * X + np.random.randn(100, 1)
```

## Linear regression using the normal equation

The most common performance measure of a regression model os the root mean squar error (RMSE). In practice, it is however simpler to minimise the mean square error (MSE):

$$
\text{MSE}(\mathbf{X}, h_{\boldsymbol{\theta}}) = \dfrac{1}{m} \sum\limits_{i=1}^{m}{(\boldsymbol{\theta}^T \mathbf{x}^{(i)} - y^{(i)})^2}
$$

and the closed form solution for finding the value of $$theta$$ minising the MSE is the **normal equation**:

$$
\hat{\boldsymbol{\theta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}
$$

### By "hand"


```python
X_b = np.c_[np.ones((100, 1)), X]  # add x0 = 1 to each instance
X_b[:5]
```


    array([[1.        , 0.76363314],
           [1.        , 1.82680741],
           [1.        , 0.20164152],
           [1.        , 1.88355561],
           [1.        , 0.73956472]])


```python
theta_best = np.linalg.inv(X_b.T.dot(X_b)).dot(X_b.T).dot(y)
theta_best
```


    array([[3.66342678],
           [3.24983346]])

### Using sklearn


```python
from sklearn.linear_model import LinearRegression
lin_reg = LinearRegression()
lin_reg.fit(X, y)
lin_reg.intercept_, lin_reg.coef_
```


    (array([3.66342678]), array([[3.24983346]]))

### Pros and cons

#### Pros

* Closed-form (no iteration required)
* Linear with regard to the number of instances in the training set
* Quick predictions

#### Cons

* All instances in the training set need to fit in memory
* O(n^3) with regards to the number of features (n x n matrix needs to be inverted where n is the number of features)

## Linear regression using batch gradient descent

Batch gradient descent is using the entire training set upon each iteration to approximate the minimum. The algorithm is as follows:
1. Fill the parameters $$\theta$$ with "random" values (random initialisation)
2. Measure the local gradient of the error function with the regards to the parameter vector $$\theta$$
3. Move in the opposite direction (note: the gradient is vector facing the direction of the steepest **ascent** and you want to descend...) by a certain step, i.e. the learning rate or $$\eta$$.
4. Repeat 2 and 3 until either the gradient has converged to zero or the maximum number of repetition has been reached.

To measure the local gradient of the error function, you need to calculate its partial derivatives: 

$$
\dfrac{\partial}{\partial \theta_j} \text{MSE}(\boldsymbol{\theta}) = \dfrac{2}{m}\sum\limits_{i=1}^{m}(\boldsymbol{\theta}^T \mathbf{x}^{(i)} - y^{(i)})\, x_j^{(i)}
$$

Giving you the gradient vector of the MSE cost function: 

$$
\nabla_{\boldsymbol{\theta}}\, \text{MSE}(\boldsymbol{\theta}) =
\begin{pmatrix}
 \frac{\partial}{\partial \theta_0} \text{MSE}(\boldsymbol{\theta}) \\
 \frac{\partial}{\partial \theta_1} \text{MSE}(\boldsymbol{\theta}) \\
 \vdots \\
 \frac{\partial}{\partial \theta_n} \text{MSE}(\boldsymbol{\theta})
\end{pmatrix}
 = \dfrac{2}{m} \mathbf{X}^T (\mathbf{X} \boldsymbol{\theta} - \mathbf{y})
$$

As part of each iteration, you will slightly adjust the parameter vector $$\theta$$ for approaching the vector minimisig the MSE, i.e. descend along the MSE until you reach the minimum: 

$$
\boldsymbol{\theta}^{(\text{new})} = \boldsymbol{\theta} - \eta \nabla_{\boldsymbol{\theta}}\, \text{MSE}(\boldsymbol{\theta})
$$


```python
eta = 0.1 # learning rate
n_iterations = 1000 
m = len(X)
theta = np.random.randn(2,1) # random initialisation

for iteration in range(n_iterations):
    gradients = 2/m * X_b.T.dot(X_b.dot(theta) - y)
    theta = theta - eta * gradients

theta
```


    array([[3.66342678],
           [3.24983346]])

### Pros and cons

### Pros

* Works also when the cost function cannot be derived
* Scales well with the number of features (faster than the normal equation)

### Cons

* Needs to be parametrized well to converge to the absolute minimum (initialisation and learning rate)
* Features need to be scaled or it would take too much time to converge
* Does not scale well to large training sets (slow as the gradient over the entire training set needs to be calculated upon each iteration)

## Linear regression using stochastic gradient descent

Stochastic in this context means random, in that, upon each iteration, the parameter vector is updated using only one random row in the set of observations. The algorith is as follows: 

1. Fill the parameters $$\theta$$ with "random" values (random initialisation)
2. Select a random observation and measure the local gradient of the error function with the regards to the parameter vector $$\theta$$
3. Move in the opposite direction by a certain step, i.e. the learning rate or $$\eta$$.
4. Repeat 2 and 3 *m* times (this is a convention, *m* being the number of observations in the training set)
5. Repeat 2 to 4 until either the gradient has converged to zero or the maximum number of repetition has been reached.

### By "hand"


```python
n_epochs = 50
m = len(X)

t0, t1 = 5, 50  # learning schedule hyperparameters
def learning_schedule(t): # to calculate eta, steps get smaller
    return t0 / (t + t1)

theta = np.random.randn(2,1)  # random initialisation

for epoch in range(n_epochs):
    for i in range(m):
        random_index = np.random.randint(m)
        xi = X_b[random_index:random_index+1]
        yi = y[random_index:random_index+1]
        gradients = 2 * xi.T.dot(xi.dot(theta) - yi)
        eta = learning_schedule(epoch * m + i)
        theta = theta - eta * gradients
        
theta
```


    array([[3.66729587],
           [3.30349553]])

### Using sklearn


```python
from sklearn.linear_model import SGDRegressor
sgd_reg = SGDRegressor(max_iter=50, tol=-np.infty, 
                       penalty=None, eta0=0.1, random_state=42)
sgd_reg.fit(X, y.ravel())
sgd_reg.intercept_, sgd_reg.coef_
```


    (array([3.64748968]), array([3.23167209]))

### Pros and cons

#### Pros

* Same as for batch gradient descent
* Faster than batch gradient descent (only the gradient at the randomly selected observation needs to be calculated upon each iteration)
* Better than batch gradient descent at escaping local minima

#### Cons

* Convergence is more irregular than for the batch gradient descent, including close to the minimum where the cost function will continue to bounce up and down - only good parameters values can be obtained but not the optimal ones
* Features need to be scaled or it would take too much time to converge
