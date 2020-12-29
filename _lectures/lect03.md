---
num: "Lecture 3"
desc: "Python Review cont. Testing, Python Errors"
ready: true
lecture_date: 2020-10-08 17:00:00.00-7:00
---

Recorded Lecture: [10_8_20](https://drive.google.com/file/d/1jXkocyP9zFFvnyYYEuZoa_gmMKFboMZX/view?usp=sharing)

# Dictionaries

* Otherwise known as TABLES or MAPS
	* Works where a unique KEY maps to a VALUE
* Gives us more precise indexing than lists

```
DICT = {<key1>:<value1>, <key2>:<value2>, ... , <keyN>:<valueN>}

D = {} # Empty dictionary. Notice the curly braces instead of []
print(D)
print(len(D))

D = {'Jane':18, 'Jen':19, 'Joe':20, 'John':21}

print(D)
print(len(D))
print("Jane's age is:", D['Jane'])

# Simple way to get key / value
for x in D:
    print(x) # Prints the key
    print(D[x]) # Prints the value
```

## Restrictions on using Dictionaries

* Keys must be an immutable type (int, str, namedtuples, tuples, ... not lists for example).
* Values can be any type (immutable or mutable) and do not have to be unique
* For our purposes, KEYS are UNIQUE. Don't define `{'Joe':17, 'Joe':18}`
	* Python is actually OK with duplicate keys in the definition, and it will keep the last key / value in the dictionary

* Example

```
value = D.pop("Joe")  # returns the value associated with Joe, error if doesn’t exist
print(value)
print(D)
value = D.get("jksldf") # returns none if doesn’t exist
print(value)

D["Jem"] = 22 # Adds element to dictionary
print(D)
```

# Testing

## Complete Test

* Complete Test: Testing every possible path through the code in every possible situation
	* Generally infeasible...
	* Imagine a simple program that takes in 4 integers and prints out the max
		* In Python3, the range of valid integers is a lot!
		* Limited to memory (unlike other languages like C++ / Java where an int type is stored in 32 bits (4 bytes)
		* The number of computations to test EVERY POSSIBLE combination of the 4 integers will take A LONG TIME to compute!

* Unit Testing: Testing individual pieces (units) of a program to ensure correct behavior

## Test Driven Development (TDD)
1. Write test cases that describe what the intended behavior of a unit of software should.
Kinda like the requirements of your piece of software
2. Implement the details of the functionality with the intention of passing the tests
3. Repeat until the tests pass.

* Imagine large software products where dozens of engineers are trying to add new features / implement optimizations all at the same time
* Having a "suite" of tests before deploying software to the public is essential
	* Someone may modify changes that work for a current version, but breaks functionality in a future version
	* Rigorous tests enable confidence in the stability in software

## Pytest
* Pytest is a framework that allows developers to write tests to check the correctness of their code
* Gradescope actually uses pytest to check the "correct" answer with students’ submissions
* We can write functions that start with test_ and the body of the function can contain assert statements (as seen in lab00)
	* Pytest will run each of these functions are report which tests passed and which tests failed
* Try and install Pytest on your computer (will use this in our examples)
Installation Guide: <https://docs.pytest.org/en/stable/getting-started.html>

* Example

Write a function `biggestInt(a,b,c,d)` that takes 4 int values and returns the largest

* Let's write our our tests first (TDD!)

```
# temp_test.py

# imports the biggestInt function from temp.py
from temp import biggestInt 

def test_biggestInt1():
    assert biggestInt(1,2,3,4) == 4
    assert biggestInt(1,2,4,3) == 4
    assert biggestInt(1,4,2,3) == 4

def test_biggestInt2():
    assert biggestInt(5,5,5,5) == 5
    # etc.

def test_biggestInt3():
    assert biggestInt(-5,-10,-12,-100) == -5
    assert biggestInt(-100, 1, 100, 0) == 100
    # etc.
```

* Now let’s write the function:

```
# temp.py

def biggestInt(a,b,c,d):
	biggest = 0
	if a >= b and a >= c and a >= d:
		return a
	if b >= a and b >= c and b >= d:
		return b
	if c >= a and c >= b and c >= d:
		return c
	else:
		return d
```

Command to run pytest on temp_test.py:
* Navigate to the folder containing temp.py and temp_test.py
* (On mac terminal): `python3 -m pytest temp_test.py`

# Python Errors

* We've probably seen Python complain before even running the program
* For example:

```
print("Start")

PYTHON!

print( Hello )
```

* In this case, the program didn’t run at all. Before anything happened, * Python basically is telling us that PYTHON! Is a syntax error.
* Before any Python script runs, it gets parsed through and there’s a simple check to make sure all expressions are legal.
* If not, then it will state an error. Note that the program is not running at this time.
* If we remove the syntactically incorrect line:

```
print("Start")
print( Hello )
```

* We get another type of error that happens WHILE the code is executing.
* Errors that happen during program execution is called a runtime error:

```
Traceback (most recent call last):
  File "/Users/richert/Desktop/UCSB/CS9/lecture.py", line 5, in <module>
    print( Hello )
NameError: name 'Hello' is not defined
```

* The above message is basically saying `Hello` is a variable that hasn’t been defined, but we’re trying to use it in a print statement.
* Syntactically, there is nothing wrong with the above lines of code (since `Hello` could be a valid variable name).
* In this case, when python tries to execute the statement `print( Hello )`, it throws an exception

# Exceptions

* Exceptions are errors that occur during program execution
* So far, when we’ve encountered these runtime errors, we’ve just noticed that the program crashes
* However, there are ways to recover from runtime errors since we can handle exceptions in our code
* In the above code segment, it’s complaining about a certain type of error called `NameError`
* There are many types of exceptions types that can occur during runtime. For example:

```
print("Start")
print (1/0)
```

* Gives us the exception:

```
Traceback (most recent call last):
  File "/Users/richert/Desktop/UCSB/CS9/lecture.py", line 5, in <module>
    print( 1/0 )
ZeroDivisionError: division by zero
```

* In this case, the type of exception that has been thrown is called a `ZeroDivisionError` because a number divided by 0 is undefined

```
print("Start")
print (‘5’ + 5)
```

* Gives us the exception:

```
Traceback (most recent call last):
  File "/Users/richert/Desktop/UCSB/CS9/lecture.py", line 5, in <module>
    print( '5' + 5 )
TypeError: can only concatenate str (not "int") to str
```

* In this case, a `TypeError` occurred since ‘+’ cannot add str types.

