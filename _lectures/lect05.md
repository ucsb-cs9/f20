---
num: "Lecture 5"
desc: "Python Classes cont."
ready: true
lecture_date: 2020-10-15 17:00:00.00-7:00
---

Recorded Lecture: [10_15_20](https://drive.google.com/file/d/1bLXGMu71ZK6E5KZ0piiQJLRQINQ_rpZ0/view?usp=sharing)


# Container Classes

* Let's continue to expand on our `Student` class from last lecture.
* Classes are also useful to organize / maintain state of a program
* Student objects are useful to organize the attributes of a single student
* But let’s imagine we are trying to write a database of various students
	* The database may be useful to search for students with certain attributes...
	* We will need to add / remove things from our database
	* We can define a class to represent a collection of courses containing students
	* Let’s set up a collection of courses such that a dictionary key is defined by the course number and the value is a list of actual student objects

```
# Courses.py
from Student import Student

class Courses:
    ''' Class representing a collection of courses. Courses are
        organized by a dictionary where the key is the
        course number and the corresponding value is
        a list of Students of this course '''

	def __init__(self):
		self.courses = {}

	def addStudent(self, student, courseNum):
		# If no perm exists…
		if self.courses.get(courseNum) == None:
			self.courses[courseNum] = [student]
		elif not student in self.courses.get(courseNum):
			self.courses[courseNum].append(student)

	def printCourses(self):
		for courseNum in self.courses:
		print("CourseNum: ", courseNum)
		for student in self.courses[courseNum]:
			student.printAttributes()
		print("---")
```

```
# lecture.py

from Student import Student
from Courses import Courses

s1 = Student("Jane", 1234567)
s2 = Student("Joe", 7654321)
s3 = Student("Jill", 5555555)

UCSB = Courses()
UCSB.addStudent(s1, "CS8")
UCSB.addStudent(s2, "CS9")
UCSB.addStudent(s3, "CS16")
UCSB.printCourses()

# Try adding the same student - should have no effect
UCSB.addStudent(s2, "CS9")
```

# Let's write pytests for the Courses class!

```
# courses_test.py
from Student import Student
from Courses import Courses

def test_addSingleStudent():
	s1 = Student("Gaucho", 1234567)
	UCSB = Courses()
	UCSB.addStudent(s1, "CS9")
	courses = UCSB.getCourses()
	assert courses == {"CS9":[s1]}

def test_addMultipleStudentsToCourse():
	s1 = Student("Gaucho", 1234567)
	s2 = Student("Jane", 7654321)
	UCSB = Courses()
	UCSB.addStudent(s1, "CS9")
	UCSB.addStudent(s2, "CS9")
	courses = UCSB.getCourses()
	assert courses == {"CS9":[s1,s2]}

def test_addStudentsToCourses():
	s1 = Student("Gaucho", 1234567)
	s2 = Student("Jane", 7654321)
	s3 = Student("Jill", 5555555)
	UCSB = Courses()
	UCSB.addStudent(s1, "CS9")
	UCSB.addStudent(s2, "CS9")
	UCSB.addStudent(s3, "CS16")
	courses = UCSB.getCourses()

	# Note: Since Python 3.7, Dictionary keys are in inserted order
	assert courses == {"CS9":[s1,s2], "CS16":[s3]}

def test_addSameStudentTwice():
	s1 = Student("Gaucho", 1234567)
	s2 = Student("Jane", 7654321)
	s3 = Student("Jill", 5555555)
	UCSB = Courses()
	UCSB.addStudent(s1, "CS9")
	UCSB.addStudent(s2, "CS9")
	UCSB.addStudent(s1, "CS9")
	UCSB.addStudent(s3, "CS16")
	courses = UCSB.getCourses()    
	assert courses == {"CS9":[s1,s2], "CS16":[s3]}
```

# Shallow vs. Deep Equality

* Python allows us to check for equality using the `==` operator for our objects
* But Python doesn’t have any knowledge of what makes a Student equal to another Student in this case
* So by default, Python uses the memory address (not the values) to determine if two objects are the same
* This is known as a **shallow equality**
* Example

```
s1 = Student("Jane", 1234567)
s2 = Student("Jane", 1234567)
print(s1 == s2) # False, doesn’t compare values!
```

* In order to provide our meaning of equality for two Student objects, we will have to define the `__eq__` method in our Student class
* In this case, we can assume two Students are the same if they have the same perm number
	* Comparing values (instead of memory addresses) is called **deep equality**

```
# Add the __eq__ method in Student.py

# s1 == s2, self is the left operand (s1), rhs is the right operand (s2)
def __eq__(self, rhs):
        return self.perm == rhs.perm
```

```
s1 = Student("Jane", 1234567)
s2 = Student("Jane", 1234567)
print(s1 == s2) # True, compares the perm values!
```
