---
layout: default
title: Debugging
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/debugging/
nav_order: 7
---

# Debugging

## Errors

### Common errors

* **SyntaxError**: syntax error, typos
* **NameError**: non-existent variable
* **TypeError**: mismatch of datatypes
* **IndexError**: trying to access an element within a <u>list</u> with an incorrect index
* **ValueError**: when a built-in operation or function receives an argument that has the right type (otherwise, see TypeError) but not the right value
* **KeyError**: try to access <u>dictionary</u> with incorrect key
* **AttributeError**: when trying to call an attribute to an object that does not exist for that object.

### Create own errors and exceptions

Use of `raise` usually with `if`.

```python
raise ValueError('blah')
raise NameError('blah')
```

## Handle errors

### Use `try`and `except`

```python
try:
    #code to try
except ERROR_TYPE_1:
    # code if trial fails 
except ERROR_TYPE_2:
    # code if trial fails 
except (ERROR_TYPE_3, ERROR_TYPE_4): # use of tuple
    # code if trial fails 
else:
    # code if trial succeeds
finally:
    # code executing no matter what

try:
    #code to try
except NameError as err:
    print(err)
```

This can for example be used when input should be re-entered by the user so long as it is not valid. You can use a `while True` loop with `break` when the input is valid. Example:

```python
while True:
    try:
        num = int(input("Please enter a number:"))
    except:
        print("That is not a number.")
    else:
        break    
```

:warning: `try` and `except` are memory intensive so use with parsimony or, if possible, use only once to encompass the entire code. 

### Use pdb

pdb = Python Debugger to handle errors that we did not except (otherwise handled with `try` and `except`). 

```python
import pdb
pdb.set_trace() #place where you think there is an issue
```

Upon executing the code, the code will stop where `set_trace()` was placed and show the next line to be executed:

* press `l` for seeing where you are in the code

* enter a variable name to access its value

* press `n` for executing the next line

* press `c` for executing the rest of the code

* press `q` for quitting

* press `p` for printing

  :warning: Do not use `a`, `b`, `c`, etc. as variable names

