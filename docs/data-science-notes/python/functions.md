---
layout: default
title: Functions
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/functions/
nav_order: 4
---

# Functions

## Basic syntax

```python
def square(a):
    return a*a

def add_values(a,b):
    return a+b
```

## Lambda syntax

```python
square = lambda a: a*a
add_values = lambda a,b: a+b
```

### map()

```python
nums = [1,2,3]
doubles = list(map(lambda num: num*2, nums)) # map(function, iterable_object)
>> [2,4,6]
```

### filter()

```python
nums = [1,2,3,4]
even_nums = list(filter(lambda num: num%2 == 0, nums)) # filter(boolean_function, iterable_object)
>> [2,4]
```

## Built-in functions

### any() and all()

`all()` returns `True` if all conditions are `True`

```python
nums = [2,4]
are_nums_all_even = all([num%2==0 for num in nums]) # [] are optional
```

`any()` returns `True` if at least one condition is `True`

```python
nums = [1,2]
is_there_an_even = any([num%2==0 for num in nums]) #[] are optional
```

### sorted()

Works on every iterable object and returns a sorted version of the object without modifying the object.

```python
nums = [2,1,3]
sorted(nums, reverse=True)
>> [3,2,1]
# VERSUS
nums.sort() # sorts in place, list-specific
```

## *args, **kwargs and parameter ordering

### *args

`*args` is a special operator we can pass to functions. This parameter collects any number of additional or extra arguments in a **tuple** called `args`.

This is particularly handy when we do not know in advance how many parameters will be passed in and we want the function to tolerate any number of arguments.

:information_source: Calling this parameter `*args` is a convention but it might be called anything as long as it starts with an asterisk.

**Example:** 

```python
def contains_purple(*args):
    if 'purple' in args: 
        return True
    return False

contains_purple(1,2,3) #False
contains_purple(1,2,3,'purple') #True

```

If the `*args` items are stored in a tuple or a list, you need to unpack them prior to using them in the function, also with an asterisk: 

```python
args_list = [1,2,3,'purple']
contains_purple(*args_list) #True
```

### **kwargs

`**kwargs` is a special operator we can pass to functions. This parameter collects any number of additional or extra **keyword** arguments (including the corresponding value) in a dictionary called `kwargs`.

:information_source: Calling this parameter `**kwargs` is a convention but it might be called anything as long as it starts with a double asterisk.

**Example:**

```python
def combine_words(word,**kwargs):
    if 'prefix' in kwargs:
        return kwargs['prefix'] + word
    elif 'suffix' in kwargs:
        return word + kwargs['suffix']
    return word

combine_words('child') #child
combine_words('child', prefix='man') #manchild
combine_words('child', suffix='ish') #childish
```

If the `**kwargs` items are stored in a dictionary, you need to unpack them prior to using them in the function, also with a double asterisk: 

```python
kwargs_list = {'prefix':'man','suffix':'ish'}
combine_words("child", **kwargs_list) #manchildish
```

### Parameter ordering

The following order should be respected for parameters:

* parameters
* *args
* default parameters
* **kwargs

This is because `*args` will use **any** parameter **in addition** to the parameters (otherwise parameters may be collected by mistake) and `**kwargs` will collect **any** keyword parameter **in addition** to the default keyword parameters (otherwise the default parameters may be collected by mistake).
