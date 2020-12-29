---
num: "Lecture 4"
desc: "Exception Handling, Python Classes"
ready: true
lecture_date: 2020-10-13 17:00:00.00-7:00
---

Recorded Lecture: [10_13_20](https://drive.google.com/file/d/12aTq5mYf9L19ZgnD9dXx_GUGV6peUC-N/view?usp=sharing)

# Handling Exceptions

The general rule of exception handling is:
* If an exception was raised in a program and nobody catches the exception error, then the program will terminate.
* But we can handle exception messages with a general structure referred to as `try` / `except`
* An example:

```
while True:
    try:
        x = int(input("Enter an int: ")) # input() prompts user for input
        break # breaks out of the current loop
    except Exception:
        print("Input was not a number type. Enter a int: ")
    print("Let's try again...")
print(x)
print("Resuming execution")
```

The flow of execution is:
* Everything within a `try` block is executed normally.
* If an exception occurs in the `try` block, execution halts and an exception message is passed to a corresponding `except` block.
* The `except` block tries to catch a certain exception type (note that `Exception` catches all types of exceptions (`NameError`, `TypeError`, `ZeroDivisionError`, etc).
* If there is a matching type in the `except` statements, then the `except` block is executed.
* Once done, program execution resumes past the `except` block(s).
* If no exceptions were caught, then the program will terminate with an error message.

## Catching Multiple Exceptions

Let’s slightly modify our code so another type of exception (`ZeroDivisionError`) may happen (in addition to entering a non-int type):

```
while True:
    try:
        x = int(input("Enter an int: "))
        print(x/0)
        break
    except ZeroDivisionError:
        print("Can't divide by zero - enter an int: ")
    except Exception:
        print("Input was not a number type. Enter a int: ")
    print("Let's try again...")
print("Resuming execution")
```

* In this case, the program will either complain that a number type was not entered, or if it was entered correctly, we’ll get a `ZeroDivisionError`
	* The program in this case will never execute "correctly"
* But the important thing to observe in this scenario is we can catch multiple exception types - depends on what type of exception was thrown
* The rule is:
	* `except` statements are checked top-down
	* The first matching exception type block is then executed
	* Then the program jumps past ALL the except statements (only one except block is executed) and code execution resumes

## Example of functions raising exceptions that are caught by the caller

```
def divide(numerator, denominator):
    if denominator == 0:
        raise ZeroDivisionError()
    return numerator / denominator

try:
    print(divide(1,1))
    print(divide(1,0))
    print(divide(2,2)) # Notice this doesn’t get executed
except ZeroDivisionError:
    print("Error: Cannot divide by zero")

print("Resuming Execution...")
```

* In this scenario, we have an exception raised in the divide function
* Since there isn’t an except statement in divide(), the exception message gets passed to the calling function
	* Since divide was called in a `try` block, then we check the except statements for the first match
	* If a match exists, then the first `except` block is executed, then all `except` blocks are skipped and execution resumes
* If an exception is raised and we NEVER handle it in an except block, then
Program will eventually crash with an error message (like we’ve seen)

# Python Objects and Classes

* Objects are a way for programmers to define their own Python types and create abstractions of things with the programming language.
* Each object may have a certain state that gets modified throughout program execution.
* Object Oriented (OO) programming is the way programs use and manipulate objects to solve problems and model real-world properties
* OO Programming is not REQUIRED
	* It’s more of a way to organize, read, maintain, test, and debug software in a manageable way
* We’ve been using objects already, for example:

```
>>> x = [1,2,3]
>>> type(x)
<class 'list'>
>>> x.count(3)
1
>>> x.count(-1)
0
>>> x.append(0)
>>> x
[3, 2, 1, 0]
```

* `count`, `append`, etc are all examples of **methods** that can be called on an object.
* **Methods** are like functions but are associated with an object
* In this case, Python already defined its own class called list that we can use, but sometimes we want to create our own specific objects for the applications we’re trying to build!

## Student Class Example

```
class Student:
    ''' Student class type that contains attributes for all students '''
    def setName(self, name):
        self.name = name

    def setPerm(self, perm):
        self.perm = perm

    def printAttributes(self):
        print("Student name: {}, perm: {}" \
        .format(self.name, self.perm))

s = Student()
s.setName("Chris Gaucho")
s.setPerm(1111111)
s.printAttributes()
```

* When defining methods, the `self` parameter represents the current object we’re calling the method on
* In the example above, s is the variable storing the created Student object.
* When we define the methods, the first parameter is the *instance* of the object stored in s
* We can then use and manipulate the state of the object with these methods using the `self` parameter

## Default Constructor

* We can provide either default values or set values of the object when constructing it through the parameter list
* In the example above, we can set an empty object without any initial attributes, which may cause an error when trying to use them

```
def __init__(self):
    self.name = None
    self.perm = None

s = Student()
s.printAttributes()
```

## Overloading Constructors

* We can even go a step further and define the attributes by putting them in the parameters when we create the object

```
def __init__(self, name, perm):
    self.name = name
    self.perm = perm

s = Student("Richert", 1234567)
s.printAttributes()
```

## Initializing default values in the constructor

```
def __init__(self, name=None, perm=None):
        self.name = name
        self.perm = perm
```

* In this case, we can pass in parameters or not. If not, then `None` will automatically be assigned to the `self.name` and `self.perm`
* Or we can pick and choose what to set. For example:

```
s = Student(perm=1234567)
s.printAttributes()

# Student name: None, perm: 1234567
```

* We used the default value of name (None) and then explicitly set the perm (1234567)

## Example of using objects in code

```
s1 = Student("Jane", 1234567)
s2 = Student("Joe", 7654321)
s3 = Student("Jill", 5555555)

studentList = [s1, s2, s3]

for s in studentList:
    s.printAttributes()
```

## Let’s write some tests!!

```
# student_test.py
from Student import Student

def test_defaultStudentCreation():
    s1 = Student()
    assert s1.name == None
    assert s1.perm == None

def test_normalStudentCreation():
    s1 = Student("Richert", 1234567)
    s2 = Student("Gaucho", 7654321)

    assert s1.name == "Richert"
    assert s1.perm == 1234567
    assert s2.name == "Gaucho"
    assert s2.perm == 7654321

def test_variableStudentAttributes():
    s1 = Student(perm=5555555)
    s2 = Student("Gaucho")

    assert s1.name == None
    assert s1.perm == 5555555
    assert s2.name == "Gaucho"
    assert s2.perm == None
```
