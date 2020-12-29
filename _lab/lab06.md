---
layout: lab
num: lab06
ready: true
desc: "Sorting Apartments"
assigned: 2020-11-15 23:59:59.59-7
due: 2020-11-22 23:59:59.59-7
---

# Introduction

In this lab, you'll have the opportunity to practice:

* Defining classes in Python
* Overloading the `==`, `<`, and `>` operators in a Python class
* Implementing an **O(n log n)** mergesort on a list of Apartment objects
* Writing functions that ensure the list of Apartment objects are in sorted order
* Testing your functionality with pytest

**Note:** It is important that you start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the week before the deadline if needed! 

For this lab, you have been hired as a realtor for an up-and-coming property management company located in Isla Vista. You are tasked with renting out as many apartments as you can. In order to do so, you will write a program that will sort apartment objects in a Python List. This allows you to show off the best apartments to possible tennants. You have decided that the three most important properties of an Apartment object are its **rent**, **meters from campus**, and **condition**. You have decided to sort Apartments as follows. First, you will organize the Apartment by increasing rent. In the event of a tie (several Apartments have the same rent), the meters from UCSB will be used to determine the Apartment's place in the list. The closer the apartment is to campus, the better. If the rent and the meters from campus are the same, then the Apartment's condition will be used to determine the Apartment's place in the list - the higher condition value, the better.

This lab will require you to define an `Apartment` class and define functions in the `lab06.py` file. Note that `labO6.py` *does not contain a class definition*.

# Instructions

You will need to create three files:
* `Apartment.py` - file containing a class definition for an Apartment object
* `lab06.py` - file containing mergesort and other functions defined in the `lab06.py` section of this lab.
* `testFile.py` - file containing pytest functions testing the overall correctness of your class definitions

There will be no starter code for this assignment, but rather the class descriptions and required methods / functions are defined in the specification below.

You should organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import / from` technique shown in lecture.

# `Apartment.py`

The `Apartment.py` file will contain the definition of an `Apartment` class. We will define the Apartment attributes as follows:

* `rent` - float that represents the rent of the Apartment
* `metersFromUCSB` - integer that represents the Apartment's distance, in meters, from UCSB
* `condition` - integer from 0 to 10 (inclusive) that represents the condition of the Apartment. The higher the condition, the better.

You should write a constructor that allows the user to construct an apartment object by passing in values for all of the fields. Your constructor should set the `rent`, `metersFromUCSB`, and the `condition` attribute to `0` by default.

* `__init__(self, rent, metersFromUCSB, condition)`

In addition to your constructor, your class definition should also support "getter" methods that can receive the state of the Apartment object:

* `getRent(self)`
* `getMetersFromUCSB(self)`
* `getCondition(self)`

You will implement the method

* `getApartmentDetails(self)`

that returns a `str` with all of the Apartment attributes. The string should contain all attributes in the following EXACT format (**Note: There is no `\n` character at the end of this string**):

```python
a0 = Apartment(1204.56, 200, 3)
print(a0.getApartmentDetails())
```

<b>Output</b>
```
(Apartment) Rent: $1204.56, Distance From UCSB: 200m, Condition: 3/10
```

* Lastly, your `Apartment` class should overload the `>`,`<`, and `==` operators. This will be used when finding the proper position of an Apartment in the list using the specifications in the **Introduction** section of this lab. In this context for example, the `<` operator will return True for `Apartment1 < Apartment2` if Apartment1 is **better than** Apartment2. We reviewed operator overloading in class and the textbook does discuss overloading Python operators. You can also refer to this reference on overloading various operators as well: [https://www.geeksforgeeks.org/operator-overloading-in-python/](https://www.geeksforgeeks.org/operator-overloading-in-python/)

# `lab06.py`

This file will contain functions that sort a list of Apartment objects, ensures that the list of Apartment objects are in ascending (best-to-worst) order according to the specification, retrives information about the n<sup>th</sup> apartment in the list, and gets the top three apartments from the sorted list. These function defintions as well as their descriptions are provided below. Note that in order for the autograder to correctly check your implementation, your function defintions must match exactly as the given specifications.

* `mergesort(apartmentList)` - Performs a mergesort on the apartmentList passed as input. Sorts the Apartment objects based on the specifications in the **Introduction** section of this lab. **Gradescope will test to ensure that your mergesort implementation's Big-O is O(n log n)**
* `ensureSortedAscending(apartmentList)` - method that returns a boolean value. Returns `True` if the apartmentList is sorted correctly in asending order. Returns `False` otherwise
* `getNthApartment(apartmentList, n)` - method that returns a string detailing the n<sup>th</sup> Apartment's rent, meters from UCSB, and condition. Note that the best apartment is considered the 1<sup>st</sup>, 2<sup>nd</sup> best apartment is considered the 2<sup>nd</sup>, etc. Also note that there is no newline at the end of the string returned by this method. You should make use of the `getApartmentDetails(self)` method you defined in `Apartment.py`. If the n<sup>th</sup> apartment does not exist, your string output should be **`"(Apartment n) DNE"`** (see **Sample Output 2** below)
* `getTopThreeApartments(sorted_apartmentList)` - method that returns a labeled, comma separated string detailing the rent, meters from UCSB, and condition of the **top three apartments** from the apartmentList. If there are **fewer** than three apartments in your list, ensure that you do not have trailing commas in your string. See the **Sample Output** below for an example of this case. You may assume that the list passed to this function is in sorted order and non-empty. Note that there is no newline at the end of the string returned by this method. You should make use of the `getApartmentDetails(self)` method you defined in `Apartment.py` 

# Sample Output 1

```python
a0 = Apartment(1204.56, 200, 3)
a1 = Apartment(1204.56, 200, 7)
a2 = Apartment(1000, 100, 9)
a3 = Apartment(1000, 214, 10)
a4 = Apartment(300, 112, 3)
a5 = Apartment(300.52, 250, 2)

apartmentList = [a0, a1, a2, a3, a4, a5]

print(getNthApartment(apartmentList, len(apartmentList)))

mergesort(apartmentList)

# The sorted apartment list is now as follows. 
# apartmentList = [a4, a5, a2, a3, a1, a0]

print(getTopThreeApartments(apartmentList))
print(getNthApartment(apartmentList, len(apartmentList)))
```

<b> Output: </b>
```
(Apartment) Rent: $300.52, Distance From UCSB: 250m, Condition: 2/10
1st: (Apartment) Rent: $300, Distance From UCSB: 112m, Condition: 3/10, 2nd: (Apartment) Rent: $300.52, Distance From UCSB: 250m, Condition: 2/10, 3rd: (Apartment) Rent: $1000, Distance From UCSB: 100m, Condition: 9/10
(Apartment) Rent: $1204.56, Distance From UCSB: 200m, Condition: 3/10
```

# Sample Output 2

```python
a0 = Apartment(2400, 50, 10)
a1 = Apartment(2200, 50, 2)

apartmentList = [a0, a1]

mergeSort(apartmentList)

# The sorted apartment list is now as follows. 
# apartmentList = [a1, a0]

print(getNthApartment(apartmentList, 3))
print(getTopThreeApartments(apartmentList))
```

<b> Output: </b>
```
(Apartment 3) DNE
1st: (Apartment) Rent: $2200, Distance From UCSB: 50m, Condition: 2/10, 2nd: (Apartment) Rent: $2400, Distance From UCSB: 50m, Condition: 10/10
```

## testFile.py pytest

This file should import your `Apartment.py` class and your `lab06.py` function. You should write tests using pytest to test if the functionality is correct. Think of various scenarios and edge cases when testing your code. Write your tests first in order to check the correctness of the `Apartment` and then `lab06.py` methods. Gradescope requires `testfile.py` to be submitted before running any autograded tests. You should write at least one test for each method in each of these classes. This includes the overloaded operators but excludes the getters.

## Submission

Once you're done with writing your recursive function definitions and tests, submit your `Apartment.py` and `lab06.py` to the `Lab06` assignment on Gradescope. There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab. There also will be tests to ensure that your mergesort in `lab06.py` runs in **O(n log n)** time. Note that if your autograder seems to be running for a really long time, your mergesort may not be running in O(n log n) time.

If the tests don't pass, you may get some error message that may or may not be obvious at this point. Don't worry - if the tests didn't pass, take a minute to think about what may have caused the error. If your tests didn't pass and you're still not sure why you're getting the error, feel free to ask your TAs or Learning Assistants.

<sup>* Lab06 created by Gautam Mundewadi and adapted by Richert Wang (F20)</sup>