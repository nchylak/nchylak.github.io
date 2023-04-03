---
layout: default
title: Iterators and generators
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/iterators-generators/
nav_order: 2
---

# Iterators and generators 

## Iterators

Iterator: an object that can be iterated upon, i.e. an object that returns data - one element at a time - when `next()` is called on it.

Iterable: an object which will return an interator when `iter()` is called on it.

`next()`: function to be called on an iterator, whereby the iterator returns the next item until a `StopIteration` error occurs.

```python
class Counter:
	def __init__(self, low, high):
		self.current = low
		self.high = high

	def __iter__(self):
		return self

	def __next__(self):
		if self.current < self.high:
			num = self.current
			self.current += 1
			return num
		raise StopIteration

for x in Counter(50,60):
	print(x) # 50, 51, 52, ..., 59
```

## Generators

**Generator**: Generators are iterators created with generator functions.

**Generator functions**: functions where instead of `return` we use `yield` for created a generator capable of returning multiple values instead of just one with regular functions. When invoked, the generator function returns a generator (it is a “generator generator”) and when we call `next()` on the generator, the generator yields the next value and pauses until `next()` is called again.

```python
def count_up_to(max):
    count = 1
    while count <= max:
        yield count # function pauses until next() is called again
        count += 1

counter = count_up_to(5)
for x in counter:
    print(x) # 1, 2, 3, 4, 5
```

Uses a lot less memory than looping through a list: 

```python
a_list = [1, 2, 3, 4, 5]
for x in a_list:
    print(x) # 1, 2, 3, 4, 5
```

Another advantage is that you can create infinite generators:

```python
def current_beat():
    beats = (1,2,3,4)
    i = 0
    while True:
        if i >= len(beats): i=0 # reinitialisation of i once i reaches 4
        yield beats[i]
        i += 1
```