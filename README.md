# Object-Oriented Programming (OOP)

## What is OOP?

**Object-Oriented Programming (OOP)** refers to programming paradigms that use objects as the fundamental building blocks. The main aim of OOP is to bind together the data (attributes) and the functions (methods) that operate on that data, so that no other part of the code can access this data except through the defined functions.

An **object** represents a real-world entity or concept within a program, combining state and behavior.

---

## Creating a Class

A class is a blueprint for creating objects. It defines attributes and methods that its objects will have.

```python
class Student:
    name = 'anonymous'

    def __init__(self):
        print("Welcome")

    def start(self):
        print("Welcome", self.name)
```

### Notes:
- `self` is a reference to the specific instance of the class. It allows methods to access and manipulate that object's attributes.
- If `self` is printed, it returns the memory location of the object.
- `__init__()` is a default constructor called automatically when an object is created.
- Methods are functions that belong to objects.

### Example Usage:

```python
s1 = Student()
print(s1)        # Output: <memory location of object>
print(s1.name)   # Output: anonymous
s1.start()       # Output: Welcome anonymous
```

Deleting an object:

```python
del s1
```

---

## Parameterized Constructor

A constructor can also accept parameters to initialize the object attributes.

```python
class Student:
    name = 'anonymous'

    def __init__(self, fullname):
        self.name = fullname

s1 = Student("karan")
print(s1.name)  # Output: karan
```

### Notes:
- Parameterized constructors require arguments when creating an object.
- Object attributes take precedence over class attributes when accessed.

---

## `@staticmethod`

Static methods do not depend on class or instance attributes. They can be called without creating an object.

Incorrect usage (will give error):

```python
class Student:
    def hello():
        print("Hello user")

s1 = Student()
s1.hello()  # It will give error
```

Correct usage:

```python
class Student:
    @staticmethod
    def hello():
        print("Hello user")

s1 = Student()
s1.hello()  # Output: Hello user
```

### Notes:
- `@staticmethod` decorator allows methods to be called without the `self` parameter.
- Useful when method logic does not depend on class or instance data.

---

## `@classmethod`

Class methods can access or modify class attributes but not instance attributes unless accessed through the instance.

### Example – Without class method:

```python
class Person:
    name = 'anonymous'

    def __init__(self, name):
        self.name = name

p1 = Person("Hero")
print(p1.name)    # Output: Hero
print(Person.name) # Output: anonymous
```

Class attribute remains unchanged.

### Method 1 – Updating class attribute using class name:

```python
class Person:
    name = 'anonymous'

    def __init__(self, name):
        Person.name = name

p1 = Person("Hero")
print(p1.name)    # Output: Hero
print(Person.name) # Output: Hero
```

### Method 2 – Using `self.__class__`:

```python
class Person:
    name = 'anonymous'

    def __init__(self, name):
        self.__class__.name = name

p1 = Person("Hero")
print(p1.name)    # Output: Hero
print(Person.name) # Output: Hero
```

### Method 3 – Using `@classmethod`:

```python
class Person:
    name = 'anonymous'

    @classmethod
    def __init__(cls, name):
        cls.name = name

p1 = Person("Hero")
print(p1.name)    # Output: Hero
print(Person.name) # Output: Hero
```

### Notes:
- `@classmethod` allows accessing class-level data.
- `@staticmethod` is used when neither class nor instance data is needed.
- Instance methods (`self`) are used when object data is required.

---

## `@property`

### Problem Scenario:

Changing an attribute after initialization doesn’t automatically update dependent attributes.

```python
class Student:
    def __init__(self, phy, chem, math):
        self.phy = phy
        self.chem = chem
        self.math = math
        self.percentage = str((self.phy + self.chem + self.math) / 3) + "%"

stu1 = Student(98, 99, 50)
print(stu1.percentage)  # Output: 82.33333333333333%
stu1.phy = 86
print(stu1.phy)        # Output: 86
print(stu1.percentage)  # Output: 82.33333333333333% (old value)
```

### Solution Using `@property`:

```python
class Student:
    def __init__(self, phy, chem, math):
        self.phy = phy
        self.chem = chem
        self.math = math
    
    @property
    def percentage(self):
        return str((self.phy + self.chem + self.math) / 3) + "%"

stu1 = Student(98, 99, 50)
print(stu1.percentage)  # Output: 82.33333333333333%
stu1.phy = 86
print(stu1.percentage)  # Output: 78.33333333333333%
```

### Notes:
- `@property` allows attributes to be calculated dynamically.
- It ensures that dependent values always reflect the current state.

---

## Private Attributes and Methods

Prefixing attributes and methods with `__` makes them private and inaccessible from outside the class.

```python
class Student:
    __age = 23  # Private attribute

    def __hello():
        print("Welcome")  # Private method

    def start(self):
        print(self.__age)

    def welcome(self):
        self.__hello()

s1 = Student()
print(s1.__age)  # It will give error
s1.__hello()     # It will give error
s1.start()       # Output: 23
s1.welcome()     # Output: Welcome
```

### Notes:
- Private attributes and methods are used to hide implementation details.
- Access is allowed only within the class.

---

## Inheritance

### What is Inheritance?

Inheritance allows a class to derive attributes and methods from another class. It promotes code reuse.

### Example – Single Inheritance

```python
class A:
    age = 23
    gender = 'M'

    def start(self):
        print("Welcome")

class B(A):
    age = 24

s1 = B()
print(s1.age)    # Output: 24
print(s1.gender) # Output: M
s1.start()       # Output: Welcome
```

### Multi-Level Inheritance

```python
class C(B):
    age = 25

s1 = C()
print(s1.age)    # Output: 25
print(s1.gender) # Output: M
s1.start()       # Output: Welcome
```

### Multiple Inheritance

```python
class D(A, B, C):
    age = 25

s1 = D()
print(s1.age)    # Output: 25
print(s1.gender) # Output: M
s1.start()       # Output: Welcome
```

---

## `super()`

The `super()` function is used to refer to the parent class and call its methods from a subclass.

```python
class Car:
    def __init__(self, type):
        self.type = type
    
    def start(self):
        print("Car stopped")

class ToyotaCar(Car):
    def __init__(self, name, type):
        super().__init__(type)
        super().start()
        self.name = name

car1 = ToyotaCar("prius", "electric")
print(car1.type)  # Output: electric
```

### Notes:
- `super()` is used to access parent class methods or attributes.
- It allows extending and customizing inherited functionality.

---

## Abstraction

**Abstraction** hides unnecessary details from the user and only shows the relevant information. It helps reduce complexity by exposing only essential features.

---

## Encapsulation

**Encapsulation** is the technique of wrapping data and methods into a single unit (class) and restricting access from outside by using private attributes or methods.

---
