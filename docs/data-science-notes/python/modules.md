---
layout: default
title: Modules
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/modules/
nav_order: 5
---

# Modules

## Built-in modules

As they are built-in Python, the do not need to be installed and can be imported directly. 

```python
import random
random.randint(1,100)
```

We can also give the module an alias so that we can only refer to it with this alias further on.

```python
import random as rd
rd.randint(1,100)
```

We can also import a specific function directly to the namespace, i.e. as the function is in the namespace we do not need to specify the module it belongs to anymore when we call it.

```python
from random import randint
randint(1,100)
```

or, also using an alias:

```python
from random import randint as magic_number_chooser
magic_number_chooser(1,100)
```

## External modules

External modules are downloaded from the Internet. These can be downloaded using `pip`.

```shell
python3 -m pip install NAME_OF_PACKAGE
```

:warning: Proceed differently in Anaconda: search for package on [Anaconda.org](www.anaconda.org) and use the Anaconda Prompt-specific command line (execute Anaconda Prompt as administrator).

Use the built-in function `help` to print the documentation of the package.

```python
help(NAME_OF_PACKAGE)
```

## Custom modules

You can write code in a script and import this code in another script with the `import` function. See below example with `fruits.py` containing the function `banama`.

```python
import fruits as fts
fts.banana()
```

Or, alternatively: 

```python
from fruits import banana
banana()
```

:warning: The script needs to be in the same directory in order to be imported directly. Otherwise, [TBC].

## `__name__` variable

Every Python file has a `__name__` variable. If the file is the main file being run, its value is `__main__`. Otherwise, i.e. the Python file is being executed due to another Python file being run, its value is the file name.

The problem is that, when importing an external module, the code within this module is automatically being run. In order to avoid this, a condition on `__name__` may be used in the script of the custom module:

```python
if __name__ =="__main__":
    # this code will only run if the file is the main file
```

