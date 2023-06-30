---
layout: default
title: Bias-variance trade-off
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/7bias-variance-trade-off/
nav_order: 7
---

# Bias-variance trade-off

A model’s generalisation error can be expressed as the sum of three very different errors.

## Bias

This part of the generalization error is due to wrong assumptions, such as assuming that the data is linear when it is actually quadratic. A high-bias model is most likely to underfit the training data.

## Variance

This part is due to the model’s excessive sensitivity to small variations in the training data. A model with many degrees of freedom (such as a high-degree polynomial model) is likely to have high variance, and thus to overfit the training data.

## Irreducible error

This part is due to the noisiness of the data itself. The only way to reduce this part of the error is to clean up the data (e.g., fix the data sources, such as broken sensors, or detect and remove outliers).
