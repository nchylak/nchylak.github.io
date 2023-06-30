---
layout: default
title: Maximum likelihood estimation
parent: Probability and statistics
grand_parent: Data science notes
permalink: /docs/data-science-notes/probability-and-statistics/mle/
nav_order: 4
---

# Maximum likelihood estimation

## Definition

**Maximum likelihood estimation** is a method that determines values for the parameters of a model. The parameter values are found such that they maximize the likelihood that the process described by the model produced the data that were actually observed.

## Approach

1. Look at the data observed and pick a possible model (e.g. if the observations are dense around the mean and not dense on the tails, pick the normal distribution).
2. Find the parameters:
   1. Calculate the total probability of observing all of the data points. If we assume the observations are independent, then the total probability of observing all of data is the product of observing each data point individually. We obtain a function with $$\mu​$$ and $$\sigma​$$ as unknown. 
   2. We want to derive this function to find $$\mu$$ and $$\sigma$$ that maximize the total probability. If the function is hard to derive, we may apply natural logarithm to it first (this is fine as ln is a monotonically increasing function).
   3. We solve for the derivative equal to zero and find maxima and minima. The parameters we are looking for are ideally the ones for the global maximum.

## Expectation-maximization algorithm

It’s more likely that in a real world scenario the derivative of the log-likelihood function is still analytically intractable (i.e. it’s way too hard/impossible to differentiate the function by hand). Therefore, iterative methods like expectation-maximization algorithms are used to find numerical solutions for the parameter estimates.

The steps of the EM algorithm are usually as follows (for a mixture of gaussians):

1. Set some initial parameter estimates on your gaussians (KNN method)
2. Assign (label) the data to one of the gaussians based on which one most likely generated the data
3. Treat the labels as being correct and then use MLE to re-estimate the parameters for the different gaussians
4. Repeat steps 2 and 3 until there is convergence.
