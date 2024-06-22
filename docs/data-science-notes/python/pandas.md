---
layout: default
title: Pandas
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/pandas/
nav_order: 10
---

# Pandas

[Documentation](http://pandas.pydata.org/pandas-docs/stable/getting_started/index.html)

## Import

```python
import pandas as pd
```

## Data types

### DataFrame

DataFrames are 2-dimensional arrays. Unlike ndarrays, dataframes can contain multiple data types.

```python
df = pd.DataFrame(data=my_data, index=idx_names, columns=col_names)

list_of_dict = [
    {'name': 'Nadia',
    'age': 30},
    {'name': 'Pierre',
    'age': 28}
]
df = pd.DataFrame(list_of_dict) # df with columns name and age
```

#### Index

`index` is used to identify rows.

```python
df.index # retrieve index
df.set_index([list_idx], inplace=True) # set index to specific value
df.reset_index() # reset index to 0, 1, 2, 3 etc.
```

#### Columns

`columns` is used to identify columns.

```python
df.columns # retrieve columns
df.columns = [list_of_cols] # set columns to specific value
```

#### Multi-index

Select several columns that, together, uniquely identify rows and set them as index to create a `MultiIndex` object.

```python
df.set_index(['date', 'language'], inplace=True)
df.sort_index(inplace=True) #  to be able to slice with a multi-index, you need to sort the index first
grouped_sum.swaplevel() # swap level of MultiIndex
grouped_sum.unstack() # unstack MultiIndex
```

### Series

Series are 1-dimensional arrays.

```
s = pd.Series(data=my_data, index=idx_names)
```

## Read

```python
df = pd.read_csv("path.csv")
```

## Subset Series/DataFrames

### Slice

#### By name

| Select by Label                 | Explicit Syntax             | Shorthand Convention   | Other Shorthand |
| ------------------------------- | --------------------------- | ---------------------- | --------------- |
| Single column from dataframe    | `df.loc[:,"col1"]`          | `df["col1"]`           | `df.col1`       |
| List of columns from dataframe  | `df.loc[:,["col1","col7"]]` | `df[["col1","col7"]]`  |                 |
| Slice of columns from dataframe | `df.loc[:,"col1":"col4"]`   |                        |                 |
| Single row from dataframe       | `df.loc["row4"]`            |                        |                 |
| List of rows from dataframe     | `df.loc[["row1", "row8"]]`  |                        |                 |
| Slice of rows from dataframe    | `df.loc["row3":"row5"]`     | `df["row3":"row5"]`    |                 |
| Single item from series         | `s.loc["item8"]`            | `s["item8"]`           | `s.item8`       |
| List of items from series       | `s.loc[["item1","item7"]]`  | `s[["item1","item7"]]` |                 |
| Slice of items from series      | `s.loc["item2":"item4"]`    | `s["item2":"item4"]`   |                 |

#### By number

| Select by integer position      | Explicit Syntax    | Shorthand Convention |
| ------------------------------- | ------------------ | -------------------- |
| Single column from dataframe    | `df.iloc[:,3]`     |                      |
| List of columns from dataframe  | `iloc[:,[3,5,6]]`  |                      |
| Slice of columns from dataframe | `df.iloc[:,3:7]`   |                      |
| Single row from dataframe       | `df.iloc[20]`      |                      |
| List of rows from dataframe     | `df.iloc[[0,3,8]]` |                      |
| Slice of rows from dataframe    | `df.iloc[3:5]`     | `df[3:5]`            |
| Single items from series        | `s.iloc[8]`        | `s[8]`               |
| List of item from series        | `s.iloc[[2,8,1]]`  | `s[[2,8,1]]`         |
| Slice of items from series      | `s.iloc[5:10]`     | `s[5:10]`            |

### Filter

```python
criteria_bool = condition # e.g. df["col1"] == 0
df.loc[criteria_bool] # filter rows where condition is met, keeps all columns
df["col1"][criteria_bool] # filter col1 for rows where condition is met
```

#### Create booleans

```python
s.str.contains() # True if string contains...
s.str.endswith() # True if string ends with...
s.str.startswith() # True if string starts with...
df.isna() # True if value missing... (returns boolean df where True = missing value)
df.notna() # inverse of df.isna()
```

#### Logical operators

| pandas                          | Python equivalent | Meaning                               |
| ------------------------------- | ----------------- | ------------------------------------- |
| `(condition A) & (condition B)` | `a and b`         | `True` if both `a` and `b` are `True` |
| `(condition A) | (condition B)` | `a or b`          | `True` if either `a` or `b` is `True` |
| `~ (condition A)`               | `not a`           | `True` if `a` is `False`              |

:exclamation: Parenthesis are required.

## Modify Series/DataFrames

:exclamation: It is good practice to use `reset_index` after sorting a data frame so that there is no gap in the index sequence.

### Drop values

```python
df.dropna(inplace=True)
df.drop_duplicates(inplace=True)
```

### Replace values

```python
df.loc[location_of_values] = new_value
df.loc[condition_on_rows, column] = new_value # replace values conditionally
df.rename(columns = {'old name': 'new name'}) #change column names
```

### Add values

```python
df[new_col] = something # adds new column
```

### Sort values

```python
df.sort_values(ascending = True, inplace = True) # sort ascending order
df_sorted = df.sort_values() # shortchut as ascending = True is default and inplace = False is default
df.sort_index() # sort by labels, default is axis = 0
```

## Analyze Series/DataFrames

### Basics

#### DataFrames

```python
df.shape # dimension of object
df.dtypes # data type per column
df.head() # see first 5 rows
df.tail() # see last 5 rows
df.info() # dimension, data types, missing values, etc.
df.count() # counts non missing values per column
df.count(axis='columns') #counts non missing values per row (yeah not a typo)
```

#### Series

```python
s.describe() # statistics about a series (mean, std, percentiles, etc.)
s.unique() # extract unique values from series
s.value_counts() # frequency table of unique non-null values in series
s.value_counts(dropna=False) # frequency table of unique values in series, incl. NA
```

### Pivot table

```python
df.pivot_table(values="col to which aggfunc is applied", index=["cols to group by"], aggfunc= np.function) # import numpy
```

### Apply

```python
df.apply(func, axis=0)
```

### Group by

```python
data_gby = sales.groupby('colour')
data_gby.groups # see the groups
for key,value in data_gby: #iterate through the groups and their values

grouped_sum = df.groupby(['A', 'B'], as_index=False)['col_to_sum'].sum() # as_index=F, i.e. the grouping columns are not set as index, otherwise MultiIndex object is created

for i, (name, group) in enumerate(groupby_object):
    # iterate through the index, groupby-value
```

### The split-apply-combine strategy

![](../../../../../../assets/images/split_apply_combine.PNG)

## Arithmetic

```python
df.sum(axis=0) # sum across columns (axis = 1 for rows)
df.sum(axis="index") # sum across columns, i.e. along rows (axis="columns" for sum across rows, i.e. along columns)
df.max()
df.min()
```
