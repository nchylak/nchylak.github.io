---
layout: default
title: NumPy
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/numpy/
nav_order: 9
---

# NumPy

[Documentation](https://docs.scipy.org/doc/numpy/user/basics.html)

#### Import

```python
import numpy as np
```

#### Create arrays

NumPy ndarrays can contain only one type. Upon conversion, the main type is picked by NumPy and items that cannot be coerced to this type are replaced by `nan`.

```python
x = np.array([2,3,1,0]) # 1-d array (vector)
x = np.array([[1,5,2,0],[2,3,1,0]]) # 2-d array (matrix)
x = np.zeros((2, 3)) # 2x3 array with zeros
x = np.random.random((2,2)) # random 2x2 array
x = np.arange(start, stop, step) # array with incremental values
```

#### Modify arrays

##### Replace values

```python
x[location_of_values] = new_value
```

##### Add values

```python
new_array = np.concatenate([array_1, array_2], axis=1) # concatenate column to column, axis=0 for row to row
x = np.expand_dims(x,axis=0) # changes dimension of array from e.g. (3,) to (1,3) - or (3,1) if axis=1 - as same dimension is required for concatenation
```

##### Sort values

```python
ordered_idx = np.argsort(a_column)
x_sorted = x[ordered_idx]
```

##### Reshape

```python
x_matrix = x_list.reshape(3,3) # shrink a list to reshape as 3x3 matrix
```

#### Subset arrays

##### Slice

```python
x[0] # first row, all columns
x[:,0] # all rows, first column
x[0,2] == x[0][2] # first row, third column
x[1:4,[0,2]] # second to fourth rows, first and third columns
x[1:7:2] # start, stop, step - if start and stop empty, starts at zero and ends at end of array
x[[0,2],[1,1]] # makes pairs x[0,1] and x[2,1]
```

##### Filter

Create a boolean array for selecting values meeting a certain criteria.

```python
blue_true = colour_col == "blue"
rows_where_blue_true = x[blue_true]
```

#### Arithmetic

```python
x.sum(axis=0) # sum across columns (axis = 1 for rows)
x.mean(axis=0) # mean across columns
x.min() #faster than built-in min() function
x.max() #faster than built-in max() function
```

#### Miscellaneous

```python
x.shape # dimension
x.dtype # data type
x.astype(type) # convert to e.g. float
copy_of_x = x.copy()
```







