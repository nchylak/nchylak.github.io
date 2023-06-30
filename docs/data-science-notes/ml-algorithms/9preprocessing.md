---
layout: default
title: Preprocessing
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/9preprocessing/
nav_order: 9
---

# Preprocessing

## Shuffle

```python
import numpy as np
shuffled_index = np.random.permutation(1000)
df_shuffled = df[shuffled_index]
```

## Train-test split

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
```
