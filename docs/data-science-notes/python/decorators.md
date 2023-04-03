---
layout: default
title: Decorators
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/decorators/
nav_order: 8
---

# Decorators

**Decorators** are functions, that wrap other functions and enhance their behavior.

Example without a decorator:

```python
def be_polite(fn):
    def wrapper():
        print("What a pleasure to meet you!")
        fn()
    return wrapper

def greet():
    print("My name is Nadia.")
    
polite_greet = be_polite(greet)
polite_greet() # What a pleasure to meet you! My name is Nadia.
```

Example with a decorator:

```python
def be_polite(fn):
    def wrapper():
        print("What a pleasure to meet you!")
        fn()
    return wrapper

@be_polite # DECORATOR
def greet():
    print("My name is Nadia.")
    
greet() # What a pleasure to meet you! My name is Nadia.
```

* the "wrapper" function can take arguments
* there does not have to be a wrapper function
