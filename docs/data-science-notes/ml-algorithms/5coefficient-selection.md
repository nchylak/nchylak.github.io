---
layout: default
title: Coefficient selection
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/5coefficient-selection/
nav_order: 5
---

# Coefficient selection

## All-in

Use all the variables, if: 

* You have prior knowledge that they are relevant
* You want to prepare for backward elimination

## Backward elimination

1. Decide on a significance level
2. Fit full model
3. Consider coefficient with highest p-value
4. If p-value > significance level, remove the variable
5. Fit the model without the variable
6. Repeat 3 to 5.

## Forward selection

1. Decide on a significance level
2. Fit all possible models with 1 variable and select the one with the lowest p-value
3. Keep this variable and fit all possible new models with an additional variable and select the one with the lowest p-value for the new regressor. If p-value < significance level, keep the model and repeat, otherwise stop and keep the last valid model.

## Bidirectional elimination

1. Select a significance level
2. Add one regressor using the forward selection algorithm
3. Remove all superfluous variables using backward elimination
4. Repeat until no new variable is added or removed.

## All possible models

1. Select a criterion of goodness of fit (e.g. AIC)
2. Construct all possible models (i.e. $$2^N-1$$ total combinations for $$N$$ variables)
3. Select the one with the best criterion.
