---
layout: default
title: OOP
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/oop/
nav_order: 1
---

# Object-oriented programming

## Definition

Object-oriented programming (OOP): method of programming that attempts to model some process or thing in the world as **class** or **object**. The goal is to encapsulate the code into logical, hierarchical groupings using classes so that one can reason about the code at a higher level.

**Class**: blueprint for objects. Classes can contain methods (functions) and attributes (similar to keys in a dictionary).

*Example: A user class with attributes (name, age, gender, etc.) and methods (change profile picture, etc.)*

**Object**: an instance constructed from a class blueprint.

## Encapsulation and abstraction

**Encapsulation** is the grouping of public and private attributes and methods into a programmatic class, making abstraction possible.

**Abstraction** is the fact of exposing only “relevant” data in a class interface, hiding private attributes and methods (aka the “inner workings”) from users, i.e. decide what attributes and methods are <u>public</u> or <u>private</u>.

By convention, private attributes and methods (i.e. that should remain invisible to the class interface) are marked with an <u>underscore</u>.

## Classes

### Define a class

By convention, classes are in <u>singular</u> form and start with a <u>capital letter</u>.

```python
class User:
	pass 
# pass is only to prevent Python from throwing an error as an indented block is expected
```

### Instantiate a class

```python
user1 = User()
```

### Class attributes

These are attributes shared by all instances of the class and by the class itself. Class attributes can therefore be accessed with: 

* `class.attribute`; or
* `self.attribute` (as class attributes are also attributes of the individual instances).

```python
class User:
    active_users = 0
    
    def __init__(self):
        User.active_users += 1
    
    def logout(self):
        User.active_users -= 1
        return "A user has logged out"
```

Class attributes can also be used for validation purposes as shown below.

```python
class User: 
    names_allowed = ["Jane", "John"]
    def __init__(self, name, age):
        if name not in User.names_allowed:
            raise ValueError("Name can only Jane or John")
        self.name = name
        self.age = age 
```

### Class methods

Class methods are methods that are not concerned wuth instances but the class itself. They are written like instance methods except for the fact that they start with the `@classmethod` decorator.

The convention is to use `cls` instead of `self`.

```python
class User:
    active_users = 0
    
    @classmethod
    def display_active_users(cls):
        return f"There are {cls.active_users} users logged in."
    
    def __init__(self):
        User.active_users += 1
    
    def logout(self):
        User.active_users -= 1
        return "A user has logged out"
```

## Instances

### Instance attributes

Classes in Python usually have a special `__init__` method which calls itself upon instantiation of an object of that the class. `return` is not obligatory within `__init__`.

The keyword `self` refers to the current instance of the class.

```python
class User: # class
	def __init__(self, name, age):
        self.name = name
        self.age = age
```

After you can instantiate an object and access its attributes:

```python
user1 = User("Nadia", 30) # object
user1.age # 30
```

### Instance methods

Methods are defined like functions within the `class` declaration.

They should always include `self ` even it is not used within the method.

Methods can use and update an object attribute.

```python
class BankAccount:
 
    def __init__(self, owner, balance = 0.0):
        self.owner = owner
        self.balance = balance
 
    def getBalance(self):
        return self.balance
 
    def withdraw(self, amount):
        self.balance -= amount
        return self.balance
 
    def deposit(self, amount):
        self.balance += amount
        return self.balance
```

### `__repr__` method

Allows to define a representation of a class’ instances (i.e. what they look like when they are turned into a string or printed on screen).

```python
def __repr__(self):
    return f"The user {self.name} is {self.age} years old."

print(user1) # "The user Nadia is 30 years old."
```

### `__dict__` method

Whenever we assign or retrieve any object attribute like `name`, Python searches it in the object's `__dict__` dictionary.

```python
user1.__dict__ # {'name': 'Nadia', '_age': 30}
```

### Instance properties

Properties can be used for modifying a class without any changes required to the client code, i.e. while ensuring backward compatibility ([useful link](https://www.programiz.com/python-programming/property)).

For example, we would like to make `age` a private attribute `_age`. The client should still be able to:

* Retrieve age with `user.age`
* Set age with `user.age = new_age`

```python
class User:
    def __init__(self, name, age):
        self.name = name
        self._age = age
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, new_age):
        self._age = new_age

user1 = User("Nadia", 30)

print(user1.age) # 30
user1.age = 29
print(user1.age) # 29
```

## Inheritance

Upon creation of a new, we can make this class inherit the properties and methods of another class (example a moderator should have the same methods of a user plus additional ones, like deleting posts). This saves typing and makes maintenance easier.

Please note the following: 

* We specify that Moderator should inherit from User as follows: `class Moderator(User):`
* `super()` refers to the parent class (instead of writing the parent class’ name)
* All methods are imported automatically but not attributes, which need to be imported with `super().__init__(list_attributes_in_common_of_two_classes)`.

```python
class User:
    def __init__(self, name, age):
        self.name = name
        self._age = age

class Moderator(User):
    def __init__(self, name, age, community):
        super().__init__(name, age)
        self.community = community
        
mod1 = Moderator("Pierre", 28, "Soccer")
print(mod1.name) # "Pierre"
```

## Polymorphism

Polymorphism is an OOP-concept describing when methods or functions can behave differently for different classes of objects. This is possible thanks to special methods (also known as “magic”), that are dunders (i.e. double-underscored named). For example:

* `len()` can be applied to lists, tuples, etc. - see magic `__len__()`
* `+` can be applied to numbers, strings, etc. - see magic `__add__()`
* `print()` can be applied to numbers, lists, dataframes, etc. - see magic `__repr__()`.

```python
class Human:
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
	def __repr__(self):
		return f"Human named {self.name} aged {self.age}"

	def __len__(self):
		return self.age

	def __add__(self, other):
		# When you add two humans together...you get a newborn baby Human!
		if isinstance(other, Human):
			return Human(name='Newborn', age=0)
		return raise TypeError("You must add another Human")
```

