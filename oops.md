

```
class student:
    def __init__(self, sid=0, sname="", m1=0, m2=0, m3=0):
        print("Student constructor called")
        self.__sid=sid
        self.__sname=sname
        self.__m1=m1
        self.__m2=m2
        self.__m3=m3
        
    def set_sid(self,sid):
        self.__sid=sid
    def set_sname(self,nm):
        self.__sname=nm
    def set_m1(self,m1):
        self.__m1=m1
    def set_m2(self,m2):
        self.__m2=m2
    def set_m3(self,m3):
        self.__m3=m3
    
    def get_sid(self):
        return self.__sid
    def get_sname(self):
        return self.__sname
    def get_m1(self):
        return self.__m1
    def get_m2(self):
        return self.__m2
    def get_m3(self):
        return self.__m3
        
    @staticmethod
    def testfunction(x):
        print("text mt function")
        
        
    def __str__(self):
       return f"sid:{self.__sid} sname:{self.__sname} m1:{self.__m1} m2:{self.__m2} m3:{self.__m3}"


        
s1=student(12,"A-man",99,99,99)

s2=student(11,"aman",9,9,9)

print(s1)

print(s2)

print(s1.get_sname())


# %%


class student:
    count=0
    
    # static method can access only static members
    # it cannot access instance variables
    @staticmethod
    def __generateId(name):
        student.count=student.count+1
        return str(student.count)+name[0:3]
    
        
    def __init__(self, sname="", m1=0, m2=0, m3=0):
        print("Student constructor called")
        student.count=student.count+1
        self.__sid=student.__generateId(sname)
        self.__sname=sname
        self.__m1=m1
        self.__m2=m2
        self.__m3=m3
        
    #def set_sid(self,sid):
        #self.__sid=sid
    def set_sname(self,nm):
        self.__sname=nm
    def set_m1(self,m1):
        self.__m1=m1
    def set_m2(self,m2):
        self.__m2=m2
    def set_m3(self,m3):
        self.__m3=m3
    
    def get_sid(self):
        return self.__sid
    def get_sname(self):
        return self.__sname
    def get_m1(self):
        return self.__m1
    def get_m2(self):
        return self.__m2
    def get_m3(self):
        return self.__m3
        
    @staticmethod
    def testfunction(x):
        print("text mt function")
        
        
    def __str__(self):
       return f"sid:{self.__sid} sname:{self.__sname} m1:{self.__m1} m2:{self.__m2} m3:{self.__m3}"





        
s1=student("Aman",99,99,99)



s2=student("vman",9,9,9)


print(s1,s2)

======================================================================================================================================

#%%

class Account:
    def __init__(self,aid=0,name="",balance=0,pin="",sque="",sans=""):
        self.__aid=aid
        self.__name=name
        self._balance=balance
        self.__pin=pin
        self.__sque=sque
        self.__sans=sans

        def set_aid(self,aid):
            self.__aid=aid
            
        def set_name(self,name):
            self.__name=name
            
        def set_balance(self,balance):
            self._balance=balance
            
        def set_pin(self,pin):
            self.__pin=pin
            
        def set_sans(self,sans):
            self.__sans=sans

        def set_sque(self,sque):
            self.__sque=sque



        #getter methods
        
        def get_aid(self):
            return self.__aid
        
        def get_name(self):
            return self.__name
        
        def get_balance(self):
            return self._balance
        
        def get_pin(self):
            return self.__pin
        
        def get_sque(self):
            return self.__sque
        
        def getsans(self):
            return self.__sans

        def __str__(self):
            
            return f"AID :{self.__aid} NAME:{self.__name} BAL:{self._balance} pin:{self.__pin} sque:{self.__sque} sans:{self.__sans}"




class Saccount(Account):
    def __init__(self,aid=0,name="",balance=0,pin="",sque="",sans="",chnum=0):
        super().__init__(aid,name,balance,pin,sque,sans)       
        self.__chequebknm=chnum
        
    def set_chequebknm(self,num):
         self.__chequebhnm=num
         
    def get_chequebknm(self,num):
         return self.__chequebhnm   
     
            
    def __str__(self):
         return super().__str__()+f"cheque bk num{self.__chequebknm}"

class Daccount(Account):
    def __init__(self,aid=0,name="",balance=0,pin="",sque="",sans="",comm=0):
        super().__init__(aid,name,balance,pin,sque,sans)       
        self.__comm=comm
        
    def set_chequebknm(self,num):
         self.__comm=num
         
    def get_chequebknm(self,num): 
         return self.__comm   
     
            
    def __str__(self):
         return super().__str__()+f"cheque bk num{self.__comm}"

#ac1=Account(12,"Z",23456,1111,"fav place","Pashan")
#ac2=Account(13,"Y",33456,2222,"fav food","egs")
 

#print(ac1)
#print(ac2)       
            
            
            
            



sac1=Saccount(12,"Z",23456,1111,"fav place","Pashan")
sac2=Saccount(13,"Y",33456,2222,"fav food","egs")


print(sac1)
print(sac2) 

===================================================================================


class A:

    def __init__(self,a1=0,a2=0):

        print("In A constructor")

        self.__a1=a1

        self.__a2=a2

    def __str__(self):

        return f"A1: {self.__a1} A2: {self.__a2}"

    

class B(A):

    

    def __init__(self,b1=0,**kwarg):

        print("In B constructor",kwarg)

        super().__init__(**kwarg)

        self.__b1=b1

    def __str__(self):

        return super().__str__()+f" B1 : {self.__b1}"

    

class C(A):

    def __init__(self,c1=0,c2=0,**kwarg):

        print("In C constructor",kwarg)

        super().__init__(**kwarg) #a21,a2,b1

        self.__c1=c1

        self.__c2=c2

    def __str__(self):

        return super().__str__()+f" C1 : {self.__c1} C2: {self.__c2}"

    

class D(B,C):

    def __init__(self,d1=0,d2=0,**kwarg):

        print("In D constructor",kwarg)

        super().__init__(**kwarg) #a1,a2,b1,c1,c2 

        self.__d1=d1

        self.__d2=d2

    def __str__(self):

        return super().__str__()+f" D1: {self.__d1} D2: {self.__d2}"


ob=D(a1=1,a2=2,b1=11,c1=21,c2=22,d1=31,d2=32)

print(D.mro()) #DFS algorithm

print(ob)


======================================================================================
```

=========================================================================================================================================
# OOP Implementation Example — `Student`

This program combines several OOP concepts into one class:

```python
class Student:
    count = 0

    @staticmethod
    def __generateId(name):
        Student.count += 1
        return name[0:3] + str(Student.count)

    def __init__(self, sname="", m1=0, m2=0, m3=0):
        self.__sid = Student.__generateId(sname)
        self.__sname = sname
        self.__m1 = m1
        self.__m2 = m2
        self.__m3 = m3
```

---

## 1. Class

```python
class Student:
```

`Student` is the **class**.

It groups together:

```text
Student
 ├── Data
 │    ├── count
 │    ├── sid
 │    ├── sname
 │    ├── m1
 │    ├── m2
 │    └── m3
 │
 └── Methods
      ├── __generateId()
      ├── __init__()
      ├── setters
      ├── getters
      └── __str__()
```

So OOP keeps **data + functions that operate on that data** together.

---

# 2. Class / Static Variable

```python
count = 0
```

This is a **class variable**.

It belongs to the class rather than to each individual student.

```text
Student
   │
   └── count = 0
```

All `Student` objects share this variable.

When a new student is created:

```python
Student.count += 1
```

the shared count increases.

```text
Student.count = 0

s1 created → count = 1
s2 created → count = 2
s3 created → count = 3
```

---

# 3. Static Method

```python
@staticmethod
def __generateId(name):
    Student.count += 1
    return name[0:3] + str(Student.count)
```

`@staticmethod` tells Python that this method does **not receive `self` or `cls` automatically**.

It doesn't need an individual Student object's data.

It works with the **class-level `count`**.

Example:

```python
Student.__generateId("Rohit")
```

Conceptually:

```text
Static method
      ↓
No self
      ↓
Doesn't depend on a particular Student object
```

### Important correction

The comment in the original code says:

```python
# static method can access only static members
```

That's a little too restrictive.

A static method can technically access **anything available in its scope** if you explicitly provide/access it. It simply does not receive `self` or `cls` automatically.

In this example, `Student.count` is accessed through the class.

---

# 4. Constructor

```python
def __init__(self, sname="", m1=0, m2=0, m3=0):
```

`__init__()` initializes each newly created Student object.

When:

```python
s1 = Student("Rohit", 88, 0, 87)
```

Python calls:

```python
__init__(s1, "Rohit", 88, 0, 87)
```

Inside it:

```python
self.__sid = Student.__generateId(sname)
self.__sname = sname
self.__m1 = m1
self.__m2 = m2
self.__m3 = m3
```

So each object gets its own instance variables.

---

# 5. Instance Variables

These:

```python
self.__sid
self.__sname
self.__m1
self.__m2
self.__m3
```

are **instance variables**.

Each Student object gets its own values.

```text
s1
 ├── sid
 ├── sname → Rohit
 ├── m1 → 88
 ├── m2 → 0
 └── m3 → 87

s2
 ├── sid
 ├── sname → Sameer
 ├── m1 → 85
 ├── m2 → 86
 └── m3 → 89
```

So:

```text
Student.count
→ Shared

self.__sname
→ Separate for every object
```

---

# 6. Encapsulation / Private Members

The variables use double underscores:

```python
self.__sid
self.__sname
self.__m1
self.__m2
self.__m3
```

This triggers **name mangling**.

They are intended to be internal implementation details of the class.

Instead of directly manipulating them, the class provides methods to access/change them.

```text
Private/internal data
       ↓
 ┌─────┴─────┐
 ↓           ↓
Getter      Setter
 ↓           ↓
Read        Modify
```

---

# 7. Setter / Mutator Methods

Example:

```python
def set_sname(self, nm):
    self.__sname = nm
```

This modifies the student's name.

Usage:

```python
s1.set_sname("Sameer")
```

Other setters:

```python
set_m1()
set_m2()
set_m3()
```

These are **mutator methods** because they modify object state.

---

# 8. Getter / Accessor Methods

Example:

```python
def get_sname(self):
    return self.__sname
```

Usage:

```python
print(s1.get_sname())
```

Other getters:

```python
get_sid()
get_m1()
get_m2()
get_m3()
```

These are **accessor methods** because they provide controlled access to object data.

---

# 9. Why No `set_sid()`?

Notice this part:

```python
# def set_sid(self, sid):
#     self.__sid = sid
```

It is commented out intentionally.

The ID is generated automatically:

```python
self.__sid = Student.__generateId(sname)
```

Therefore, the program does not provide a setter for `sid`.

Conceptually:

```text
sid
 ↓
Generated automatically
 ↓
No setter
 ↓
Cannot normally be changed through the public API
```

This is an example of making a property effectively **read-only through the class interface**.

---

# 10. Member / Instance Methods

Methods such as:

```python
def set_sname(self, nm):
def set_m1(self, m1):
def get_sname(self):
def get_m1(self):
```

are **instance methods**.

They receive:

```python
self
```

which represents the current object.

For example:

```python
s1.set_sname("Sameer")
```

is conceptually:

```python
Student.set_sname(s1, "Sameer")
```

So `self` tells the method **which Student object to work on**.

---

# 11. `__str__()` — Object Representation

```python
def __str__(self):
    return f"Sid: {self.__sid} sname:{self.__sname} ..."
```

`__str__()` is a special/dunder method.

It defines what should be displayed when the object is converted to a string.

Therefore:

```python
print(s1)
```

automatically calls approximately:

```python
s1.__str__()
```

Instead of getting an unhelpful default object representation, you get:

```text
Sid: Roh1 sname:Rohit m1:88 m2:0 m3:87
```

---

# 12. Creating Objects

```python
s1 = Student(sname="Rohit", m1=88, m3=87)

s2 = Student("Sameer", 85, 86, 89)
```

Here:

```text
Student → Class

s1 → Object 1
s2 → Object 2
```

Both objects use the same class definition but contain different instance data.

---

# 13. Complete OOP Flow

The entire example can be visualized as:

```text
                 Student CLASS
                       │
        ┌──────────────┴──────────────┐
        │                             │
  Class Variable                 Methods
   count = 0                         │
        │                    ┌───────┼────────┐
        │                    │       │        │
        │                 Constructor Getter Setter
        │                    │       │        │
        │                    └───────┼────────┘
        │                            │
        ↓                            ↓
  Shared by objects           Operate on objects
        │
   ┌────┴────┐
   ↓         ↓
  s1         s2
   │          │
   ↓          ↓
Instance    Instance
Variables   Variables
```

---

# 14. What OOP Concepts Are Demonstrated?

| Code                   | OOP Concept                        |
| ---------------------- | ---------------------------------- |
| `class Student`        | Class                              |
| `s1`, `s2`             | Objects                            |
| `self.__sname`         | Instance variable                  |
| `count`                | Class/static variable              |
| `__init__()`           | Constructor / initializer          |
| `set_sname()`          | Mutator / Setter                   |
| `get_sname()`          | Accessor / Getter                  |
| `__generateId()`       | Static method                      |
| `__str__()`            | Special/dunder method              |
| `self`                 | Current object                     |
| `__variable`           | Name-mangled internal attribute    |
| Getter + Setter design | Encapsulation                      |
| No `set_sid()`         | Read-only through public interface |

---

# ⭐ Rapid Revision — This Example

```text
class Student
      ↓
   Blueprint
      ↓
 ┌───────────────┐
 │ count         │ ← Class variable (shared)
 │ __sid         │ ← Instance variable
 │ __sname       │ ← Instance variable
 │ __m1, __m2... │ ← Instance variables
 └───────────────┘
      │
      ├── __init__()        → Initialize object
      ├── __generateId()    → Static method
      ├── get_*()           → Accessors / Getters
      ├── set_*()           → Mutators / Setters
      └── __str__()         → String representation
```

### Core idea

```text
OOP = Data + Behavior + Controlled Access

Data
 ↓
Instance/Class variables

Behavior
 ↓
Methods

Controlled Access
 ↓
Getters / Setters / Encapsulation
```

This single `Student` example demonstrates the **core implementation of OOP in Python**: creating a class, creating objects, storing instance/class data, initializing objects, defining methods, using static methods, controlling access through getters/setters, and customizing object behavior with a dunder method.


=================================================================================================================================

# Python OOP — Part 2: Inheritance, Polymorphism & Advanced OOP

## 1. Inheritance

**Inheritance** allows a child class to reuse and extend the functionality of a parent class.

```python
class Animal:
    def speak(self):
        print("Animal sound")


class Dog(Animal):
    def bark(self):
        print("Woof")


d = Dog()

d.speak()   # inherited
d.bark()    # own method
```

```text
Animal (Parent)
     ↓
   Dog (Child)
```

### Why use it?

* Code reuse
* Extend existing classes
* Create specialized versions of a class
* Establish relationships between classes

---

# 2. Method Overriding

A child class can provide its **own implementation** of a method inherited from the parent.

```python
class Animal:
    def speak(self):
        print("Animal sound")


class Dog(Animal):
    def speak(self):
        print("Woof")


d = Dog()
d.speak()
```

Output:

```text
Woof
```

The child's `speak()` **overrides** the parent's version.

---

# 3. `super()`

`super()` is used to access functionality from the parent class.

```python
class Animal:
    def __init__(self, name):
        self.name = name


class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed


d = Dog("Bruno", "Labrador")
```

Here:

```python
super().__init__(name)
```

calls the parent constructor.

### Also useful for methods

```python
class Animal:
    def speak(self):
        print("Animal sound")


class Dog(Animal):
    def speak(self):
        super().speak()
        print("Woof")
```

Output:

```text
Animal sound
Woof
```

---

# 4. Types of Inheritance

### Single Inheritance

```text
A
↓
B
```

```python
class A:
    pass

class B(A):
    pass
```

### Multilevel Inheritance

```text
A
↓
B
↓
C
```

```python
class A:
    pass

class B(A):
    pass

class C(B):
    pass
```

### Multiple Inheritance

One child inherits from multiple parents.

```text
 A       B
  \     /
    \ /
     C
```

```python
class A:
    def method_a(self):
        print("A")


class B:
    def method_b(self):
        print("B")


class C(A, B):
    pass


obj = C()

obj.method_a()
obj.method_b()
```

### Hierarchical Inheritance

Multiple children inherit from one parent.

```text
       Animal
       /    \
     Dog    Cat
```

---

# 5. Method Resolution Order — MRO

When multiple inheritance is used, Python needs to determine **which method should be called first**.

It uses **MRO — Method Resolution Order**.

```python
class A:
    def show(self):
        print("A")


class B(A):
    def show(self):
        print("B")


class C(A):
    def show(self):
        print("C")


class D(B, C):
    pass


d = D()

d.show()
```

Check the MRO:

```python
print(D.mro())
```

or:

```python
print(D.__mro__)
```

Conceptually:

```text
D → B → C → A → object
```

Python follows this order when searching for methods.

---

# 6. Polymorphism

**Polymorphism = same interface, different behavior.**

Different classes can provide the same method name but behave differently.

```python
class Dog:
    def speak(self):
        return "Woof"


class Cat:
    def speak(self):
        return "Meow"


animals = [Dog(), Cat()]

for animal in animals:
    print(animal.speak())
```

Output:

```text
Woof
Meow
```

The code doesn't need to know whether the object is a `Dog` or `Cat`.

---

# 7. Duck Typing

Python commonly uses **duck typing**.

> If an object provides the required behavior, Python can use it.

```python
class Dog:
    def speak(self):
        return "Woof"


class Robot:
    def speak(self):
        return "Beep"


def make_sound(obj):
    print(obj.speak())


make_sound(Dog())
make_sound(Robot())
```

`Dog` and `Robot` don't need to inherit from the same class.

Python cares that both provide:

```python
speak()
```

### Remember

```text
Traditional OOP:
"What type is this object?"

Python:
"What can this object do?"
```

---

# 8. Abstract Base Classes — ABC

An **abstract class** defines a common interface that child classes must implement.

```python
from abc import ABC, abstractmethod


class Animal(ABC):

    @abstractmethod
    def speak(self):
        pass


class Dog(Animal):

    def speak(self):
        return "Woof"


dog = Dog()
print(dog.speak())
```

Trying to instantiate the abstract class directly:

```python
# Animal()
```

raises an error because `speak()` has not been implemented.

### Purpose

```text
Abstract Class
      ↓
Defines required methods
      ↓
Child classes implement them
```

Useful for:

* APIs
* Frameworks
* Plugin systems
* Large applications
* Defining common interfaces

---

# 9. Composition

**Composition = one object contains another object.**

Instead of inheritance:

```text
Car IS-A Vehicle
```

composition represents:

```text
Car HAS-A Engine
```

Example:

```python
class Engine:
    def start(self):
        print("Engine started")


class Car:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()


car = Car()

car.start()
```

```text
Car
 └── Engine
```

### Important

Prefer composition when the relationship is **"has-a"** rather than **"is-a"**.

---

# 10. Operator Overloading

Python allows classes to define how operators behave with their objects.

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Point(
            self.x + other.x,
            self.y + other.y
        )


p1 = Point(2, 3)
p2 = Point(4, 5)

p3 = p1 + p2

print(p3.x, p3.y)
```

The expression:

```python
p1 + p2
```

causes Python to use:

```python
p1.__add__(p2)
```

### Common operator dunder methods

```text
__add__     → +
__sub__     → -
__mul__     → *
__truediv__ → /
__eq__      → ==
__lt__      → <
__gt__      → >
__le__      → <=
__ge__      → >=
```

---

# 11. Important Dunder Methods

Dunder methods allow objects to interact naturally with Python's built-in operations.

### `__str__()`

Controls human-readable output:

```python
class Student:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f"Student: {self.name}"


s = Student("Ash")

print(s)
```

---

### `__repr__()`

Provides a more developer-oriented representation.

```python
class Student:
    def __init__(self, name):
        self.name = name

    def __repr__(self):
        return f"Student({self.name!r})"
```

---

### `__len__()`

Makes `len(object)` work:

```python
class Team:
    def __init__(self, members):
        self.members = members

    def __len__(self):
        return len(self.members)


team = Team(["A", "B", "C"])

print(len(team))
```

---

### `__contains__()`

Controls the `in` operator:

```python
class Team:
    def __init__(self, members):
        self.members = members

    def __contains__(self, name):
        return name in self.members


team = Team(["Ash", "John"])

print("Ash" in team)
```

---

# 12. Callable Objects — `__call__()`

An object can be made callable like a function.

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, value):
        return value * self.factor


double = Multiplier(2)

print(double(5))
```

Output:

```text
10
```

Normally:

```python
double(5)
```

looks like a function call, but `double` is actually an object.

Python calls:

```python
double.__call__(5)
```

Useful for:

* Function-like objects
* Callbacks
* Configurable behavior
* Decorator implementations

---

# 13. `@property`

`@property` allows a method to be accessed like an attribute.

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def area(self):
        return 3.14 * self.radius ** 2


c = Circle(5)

print(c.area)
```

Notice:

```python
c.area
```

instead of:

```python
c.area()
```

### With Setter

```python
class Person:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value >= 0:
            self._age = value
```

Useful for:

* Validation
* Read-only attributes
* Computed attributes
* Controlled access

---

# 14. Dataclasses

When a class mainly stores data, `dataclass` can automatically generate common methods.

```python
from dataclasses import dataclass


@dataclass
class Student:
    name: str
    age: int
    marks: int


s = Student("Ash", 20, 90)

print(s)
```

A dataclass automatically provides useful functionality such as:

```text
__init__()
__repr__()
__eq__()
```

Useful for **data/model classes**.

---

# 15. `__new__()` vs `__init__()`

Two important object lifecycle methods:

```text
__new__()
   ↓
Creates the object

__init__()
   ↓
Initializes the object
```

Example:

```python
class Student:

    def __new__(cls, name):
        print("Creating object")
        return super().__new__(cls)

    def __init__(self, name):
        print("Initializing object")
        self.name = name


s = Student("Ash")
```

Output:

```text
Creating object
Initializing object
```

Usually you only need `__init__()`.

`__new__()` becomes useful for advanced object creation patterns such as **immutable types and custom object creation**.

---

# 16. `__init_subclass__()`

Runs when a class is subclassed.

```python
class Base:

    def __init_subclass__(cls):
        print("New subclass:", cls.__name__)


class Child(Base):
    pass
```

Output:

```text
New subclass: Child
```

Useful for:

* Frameworks
* Plugin systems
* Automatically registering subclasses
* Enforcing class-level rules

---

# 17. OOP Relationships

Remember these three relationships:

```text
IS-A
 ↓
Inheritance

HAS-A
 ↓
Composition

BEHAVES-LIKE
 ↓
Duck typing / Polymorphism
```

Example:

```text
Dog IS-A Animal

Car HAS-A Engine

Dog BEHAVES-LIKE anything
that provides speak()
```

---

# ⭐ OOP Part 2 — Rapid Revision

```text
Inheritance
→ Reuse / extend parent class

Overriding
→ Child replaces parent method

super()
→ Access parent functionality

Multiple inheritance
→ One class inherits from multiple classes

MRO
→ Order Python uses to find methods

Polymorphism
→ Same interface, different behavior

Duck typing
→ Behavior matters more than explicit type

ABC
→ Define required interface

Composition
→ Object contains another object

Operator overloading
→ Customize +, -, ==, <, etc.

Dunder methods
→ Customize Python's built-in behavior

__call__
→ Make object callable

@property
→ Attribute-like controlled access

@dataclass
→ Convenient data-focused classes

__new__
→ Create object

__init__
→ Initialize object

__init_subclass__
→ Customize subclass creation
```

# Part 1 + Part 2 — Complete OOP Map

```text
                         OOP
                          │
          ┌───────────────┴────────────────┐
          ↓                                ↓
       PART 1                           PART 2
          │                                │
   Core Concepts                     Advanced Concepts
          │                                │
   ├─ Class/Object                    ├─ Inheritance
   ├─ Instance Variables              ├─ Overriding
   ├─ Class Variables                 ├─ super()
   ├─ Constructor                     ├─ Multiple Inheritance
   ├─ Instance Methods                ├─ MRO
   ├─ Getters/Setters                 ├─ Polymorphism
   ├─ Static Methods                  ├─ Duck Typing
   ├─ Class Methods                   ├─ Abstraction / ABC
   ├─ Encapsulation                   ├─ Composition
   └─ Abstraction                     ├─ Operator Overloading
                                      ├─ Dunder Methods
                                      ├─ __call__
                                      ├─ @property
                                      ├─ Dataclasses
                                      ├─ __new__
                                      └─ __init_subclass__
```

**Core distinction to remember:**

> **Inheritance** = reuse through an **IS-A** relationship.
> **Composition** = building objects through a **HAS-A** relationship.
> **Polymorphism** = different objects responding to the **same interface**.
> **Duck typing** = Python often cares about **what an object can do**, not what class it belongs to.


==============================================================================================================
