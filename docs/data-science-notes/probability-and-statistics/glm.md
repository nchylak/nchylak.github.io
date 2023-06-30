---
layout: default
title: GLM
parent: Probability and statistics
grand_parent: Data science notes
permalink: /docs/data-science-notes/probability-and-statistics/glm/
nav_order: 3
---

# GLM

## General

$$g(Y)$$ = model = $$X\beta$$ + $$\epsilon$$ where

* $$X&beta;$$: systematic component
* $$Y$$: random component
* $$g$$: link function, i.e. the link between the systematic and the random component, verifies:
  * $$E(Y)$$= $$\mu$$ = $$g^{-1}(X\beta)$$ 

**Maximum likelihood estimation** is used to estimate the parameters $$\beta$$ and not ordinary least squares - therefore, a large sample is required.

## Logistic regression

Logistic regression is a type of GLM where: 

* Random component:  Y follows a binomial distribution
* Link: $$logit(p) = ln(p / (1-p))$$

So, $$\pi / (1- \pi) = exp(\beta_0) * exp(\beta_1x_1) * ...$$

## Poisson regression

Type of GLM where: 

- Random component:  Y follows a poisson (count) distribution
- Link: $$ln(\lambda)$$

So, $$\lambda = exp(\beta_0) * exp(\beta_1x_1) * ...$$





