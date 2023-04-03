---
layout: default
title: Lists and dictionaries
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/lists-dictionaries/
nav_order: 3
---

# Lists and dictionaries

## Lists

### Create empty list

```python
a_list = []
```

### Create non-empty list

```python
a_list = ["apple", "cherry", "orange"]
```

### Add/remove elements

```python
a_list.append("tomato") # add
a_list.remove("apple") # remove first occurence of "apple"
a_list = [x for x in a_list if x!= "apple"] # remove all ocurrences of "apple"
```

### Iterate over a list

```python
for index, value in enumerate(a_list):
    # iterates over index-value couples
```

### List comprehension

```python
a_list = [x for x in a_list if x!= "apple"] # example above
```

## Dictionaries

Data structure that consists in key value pairs.

### Create empty dictionary

```python
a_dict = {}
```

### Create non-empty dictionary

```python
a_dict = {"Jan":1, "Feb":2, "Mar":3}
```

### Add/remove elements

```python
a_dict["Apr"] = 4 # add
```

### Access values

```python
a_dict["Jan"] # 1
```

### Iterate over a dictionary

```python
for value in a_dict.values():
    # interates over values (1, 2, 3, etc.)
for key in a_dict.keys():
    #iterates over keys ("Jan", "Feb", etc.)
for key, value in a_dict.items():
    # interates over keys and values
```

### Test existence

```python
if KEY in a_dict:
    # test for existence of a key
if VALUE in a_dict.values():
    # test for existence of a value
```

### Methods

```python
a_dict.clear() # empty a dictionary
a_dict.copy() # clone a dictionary
{}.fromkeys(['name', 'email', 'phone'], 'unknown') # create key-value pairs from comma separated values, usually used to pass in default values
a_dict.get(KEY) # either gets the value is key exists or None if it does not
a_dict.pop(KEY) # deletes the key and the corresponding value and returns the value
a_dict.popitem() # takes no argument and deletes key-value pair randomly and returns the key-value pair removed
dico_1.update(dico_2) # adds content of dico_2 to dico_1 (or updates values if a key is in both)
```

### Dictionary comprehension

```python
num = {'one':1, 'two':2, 'three':3}
num_squared = {key: value**2 for key, value in num.items()}
>> {'one':1, 'two':4, 'three':9}
num_squared_2 = {num: num**2 for num in [1,2,3]}
>> {1:1, 2:4, 3:9}
odd_num = {key: ('even' if value%2==0 else 'odd') for key, value in num.items()}
>> {'one':'odd', 'two':'even', 'three':'odd'}
```

