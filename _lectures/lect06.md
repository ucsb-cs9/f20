---
num: "Lecture 6"
desc: "Operator Overloading, Inheritance"
ready: true
lecture_date: 2020-10-20 17:00:00.00-7:00
---

Recorded Lecture: [10_20_20](https://drive.google.com/file/d/1eIVZ6OkbYcvkA7pUEgBUzkc4h8sWJEN1/view?usp=sharing)

# Operator Overloading

* We can define our own behavior for common operators in our classes
	* What does it mean if two student objects are equal (we defined it to mean perm numbers are equal)?
	* Or what does it mean to add (+) two students together?
	* Python allows us to define the functionality for operators!

## Defining `__str__` 

* When printing our defined objects, we may get something unusual. For example:

```
from Student import Student

s1 = Student("Gaucho", 1234567)
s2 = Student("Jane", 5555555)
print(s1) <Student.Student object at 0x7fd5380d8e80>
```

* All objects can be printed, but Python wouldn’t know what to print for user-defined objects like Student
* So it just prints the memory address (the `0x...`) of where the object exists in memory
* In order to provide our own meaning of what Python should display when printing an object like Student, we will need to define a special `__str__` method in our Student class:

```
def __str__(self):
	''' returns a string representation of a student '''
	return "Student name: {}, perm: {}".format(self.name, self.perm) 
```

* Python will now use the return value of the `__str__` method when determining what to display in the print statement
	* Now the `print(s1)` statement outputs `Student name: Gaucho, perm: 1234567`

## Overriding the '+' operator

* What would it mean to add (+) two students together?
* Maybe we can return a list collection ... could be useful ... maybe?

```
def __add__(self, rhs):
	''' Takes two students and returns a list containing these two
		students '''
    return [self, rhs]
```
```
x = s1 + s2 # returns a list of s1 + s2
print(type(x)) # list type

for i in x:
	print(i)

# Output of for loop
# Student name: Gaucho, perm: 1234567
# Student name: Jane, perm: 5555555
```

## Overloading the '<=' and '>=' operator

* Example:

```
def __le__(self, rhs):
	''' Takes two students and returns True if the
		student is less than or equal to the other
		student based on the lexicographical order
		of the name '''
	return self.name.upper() <= rhs.name.upper()

def __ge__(self, rhs):
	''' Takes two students and returns True if the
		student is greater than or equal to the other
		student based on the lexicographical order
		of the name '''
	return self.name.upper() >= rhs.name.upper()
```
```
print(s1 <= s2) # True
print(s1 >= s2) # False
print(s1 == s2) # False
print(s1 < s2) # ERROR, we didn’t define the __lt__ method
```

* Good article on this as well as a list of common operators we can overload: <https://thepythonguru.com/python-operator-overloading/>

# Inheritance

* Let's write an `Animal` class and see what inheritance looks like in action:

```
# Animal.py
class Animal:
	''' Animal class type that contains attributes for all animals '''

def __init__(self, species=None, name=None):
	self.species = species
	self.name = name

def setName(self, name):
	self.name = name

def setSpecies(self, species):
	self.species = species

def getAttributes(self):
	return "Species: {}, Name: {}".format(self.species, self.name)

def getSound(self):
	return "I'm an Animal!!!"
```

* and now let's define a `Cow` class that inherits from `Animal`

```
# Cow.py
class Cow(Animal):
    # Available method for the Cow Class 
    def setSound(self, sound):
        self.sound = sound
```
```
c = Cow("Cow", "Betsy")
print(c.getAttributes())
c.setSound("Moo") # Sets a Cow sound attribute to "Moo"
print(c.getSound()) # I’m an Animal!!! (calls the Animal.getSound method)
```

* Note that the Cow’s constructor (`__init__`) was inherited by the class `Cow` as well as the `getAttributes()` method
* Also note that we didn’t need to define the `getSound()` method since it was inherited from `Animal`
* But in this case, this inherited method may not be what we want.
* So we can redefine its functionality in the Cow class!

```
# in Cow class
def getSound(self):
	return "{}!".format(self.sound + ' ' + self.sound)
```

* We changed the `getSound()` method in the `Cow` class, so in this case our `Cow` class overrode the `getSound()` method of `Animal`
* So now, cow objects will use its own version of `makeSound()`, not the version that was inherited from `Animal`, as seen below:

```
c = Cow("Cow", "Betsy")
c.setSound("Moo") # Sets a Cow sound to "Moo"
print(c.getSound()) # Moo Moo!
```

* We can still create `Animal` objects, and `Animal` objects will still use its own version of `getSound()`

```
a = Animal("Unicorn", "Lala")
print(a.getAttributes())
print(a.getSound()) # I’m an Animal!!!
```

<b>Note:</b> The constructed object type will dictate which method in which class is called.
* It first looks at the <b>constructed object type</b> and checks if there is a method defined in that class. If so, it uses that
* If the constructed object doesn’t have a method definition in its class, then it checks the parent(s) it inherited from, and so on ...
* If there is no matching method call, then an error happens

## Extending Superclass Methods

* Some terminology:

* `Animal` in the previous example can be referred to as the <b>Base / Parent / Super class</b>
* `Cow` can be referred to as the <b>Sub / Child / Derived class</b>
* In the example above, we overrode the `makeSound` method from `Animal`
* However, sometimes we only want to extend the functionality, not completely replace it.
	* It is possible to override methods and still use the inherited functionality by calling the `super()` class methods
	* So in this case, we override the method in the child class, but we extend the base class’ functionality by using it in the child class’ overwritten method
	* Example:

```
class Cow(Animal):
	def getSound(self):
		s = "Using Super class makeSound method\n"
		s += Animal.getSound() + "\n" # Uses Animal.makeSound method
		s += "Extending it with our own makeSound functionality" + "\n"
		s += "{}!!!".format(self.sound, self.sound)
		return s

# Output:
# Using super class makeSound method
# I'm an Animal!!!
# Extending it with our own makeSound functionality
# Moo!!!
```
