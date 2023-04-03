---
layout: default
title: Scikit Learn
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/sklearn/
nav_order: 11
---

# Scikit Learn

## Create train/test dataset

```python
from sklearn.model_selection import train_test_split
train_set, test_set = train_test_split(df, test_size=0.2, random_state=42)
```

## Simple linear regression

```python
from sklearn.linear_model import LinearRegression
model_deg_1 = LinearRegression()
X_train = np.array(train_set['YearsExperience']).reshape(-1,1)
y_train_actual = np.array(train_set['Salary']).reshape(-1,1)
model_deg_1.fit(X_train, y_train_actual)
model_deg_1.intercept_, model_deg_1.coef_
```

## Polynomial regression

```python
from sklearn.preprocessing import PolynomialFeatures
# Get X and y as np.array
X = np.array(cols_X).reshape(-1,1)
y_actual = np.array(col_y).reshape(-1,1)
# Adding polynomial features
poly_features = PolynomialFeatures(degree=2, include_bias=False) # adds for each x an x**2, include_bias=False for not adding 1 to the array
X_poly = poly_features.fit_transform(X)
```

## Evaluate model

### Mean squared error

```python
from sklearn.metrics import mean_squared_error
mse_train = mean_squared_error(y_train_actual, y_train_predicted)
rmse_train = mse_train**(1/2)
```

### Mean absolute error

```python
from sklearn.metrics import mean_absolute_error
mae = mean_absolute_error(y_train_actual, y_train_predicted)
```

### Cross validation

```python

```

### ROC AUC

```python
roc_auc_score(y_train_actual, y_train_predicted)
```

## Impute missing values

```python
from sklearn.preprocessing import Imputer
imputer = Imputer(strategy='median') # impute median value to NAs
df_no_na = imputer.fit_transform(df) # df needs to contain only numerical values!!
# OR
imputer.fit(df) # first fit
df_no_na = imputer.transform(df) # then transform
```

## Encode categorical values

### Nominal values (one-hot encoding)

```python
from sklearn.preprocessing import OneHotEncoder
cat_encoder = OneHotEncoder(sparse=False)
df_cat_1hot = cat_encoder.fit_transform(df_cat)

cat_encoder.categories_ # retrieve the categories
cat_encoder.get_features_names(X_train.columns) # retrieve the category names
```

### Ordinal values

```python
from sklearn.preprocessing import OrdinalEncoder
cat_encoder = OrdinalEncoder()
df_cat_enc = cat_encoder.fit_transform(df_cat)

cat_encoder.categories_ # retrieve the categories
cat_encoder.get_feature_names(X_train.columns)
```

## Scaling

```
from sklearn.preprocessing import StandardScaler
```

