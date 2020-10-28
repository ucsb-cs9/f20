---
num: "Lecture 8"
desc: "Runtime Analysis cont."
ready: true
lecture_date: 2020-10-27 17:00:00.00-7:00
---

Recorded Lecture: [10_27_20](https://drive.google.com/file/d/1y73FGIpD5V5YhjV8FyOIfPGSqDoaIaBM/view?usp=sharing)

# Python Lists vs. Python Dictionaries

* Let's observe the performance between lists and dictionaries with an example.
* The following program counts the number of words in a file using a list and dictionary
	* They do the same thing, but the performance is vastly different...
	* `wordlist.txt` : File containing a collection of words, one per line.
		* <https://ucsb-cs8.github.io/m19-wang/lab/lab07/wordlist.txt>
	* `PeterPan.txt` : You can download classic novels from the Gutenberg Project as a .txt file!
		* <https://www.gutenberg.org/ebooks/16>

```
# Set up our data structures
DICT = {}
infile = open("wordlist.txt", 'r')
for x in infile: # x goes through each line in the file
	DICT[x.strip()] = 0 # Creates an entry in DICT with key x.strip() and value 0
print(len(DICT))
infile.close() # close the file after we’re done with it.

WORDLIST = []
for y in DICT: # put the DICT keys into WORDLIST
	WORDLIST.append(y)
print(len(WORDLIST))

# Algorithm 1 - Lists
from time import time
start = time()
infile = open("PeterPan.txt", 'r')
largeText = infile.read()
infile.close()
counter = 0
words = largeText.split()
for x in words:
	x = x.strip("\"\'()[]{},.?<>:;-").lower()
	if x in WORDLIST:
		counter += 1
end = time()
print(counter)
print("Time elapsed with WORDLIST (in seconds):", end - start)

# Algorithm 2 - Dictionaries
start = time()
infile = open("PeterPan.txt", 'r')
largeText = infile.read()
infile.close()
words = largeText.split()
counter = 0
for x in words:
	x = x.strip("\"\'()[]{},.?<>:;-").lower()
	if x in DICT: # Searching through the DICT
		counter += 1
end = time()
print(counter)
print("Time elapsed with DICT (in seconds):", end - start)
```

* Python lists are efficient if we know the index of the item we're looking for
	* The reason why adding to the front of the list is costly is because lists have to "make room" for the element to be at index 0
		* All existing elements of the list need to shift one index up in order for the inserted element to be placed at index 0
	* Adding to the end of the list is not nearly as costly because no shifting of existing elements needs to occur
* For this example, since we are looking for the value in the list (without knowing the index), we are checking through the entire WORDLIST for every word in `PeterPan.txt`

* Python dictionaries are efficient when looking up a key value
	* Dictionary keys are actually stored in an underlying list
	* Keys are converted into a numerical value, which is passed into a **hash function**
		* The purpose of the hash function is to output the index for the underlying list based on the key value
		* We do not have to scan the underlying list structure since a key will always be placed into a specific location in the the underlying list
		* We won't go into the implementation now, but we'll revist this in more detail later
* There are MANY ways to solve a problem in programming, but understanding how certain tools work and making the best decisions is important!

## Binary Search Algorithm

* **Binary Search** is a useful algorithm to search for an item in a collection **IF THE ITEMS ARE IN SORTED ORDER**
	* Since the collection is in sorted order, we can check the middle to see if the item we're looking for is there
		* If the middle element is the one we're looking for, then great!
		* If the item we're looking for is < the middle element, then we don't have to search the right side of the collection
		* If the item we're looking for is > the middle element, then we don't have to search the left side of the collection
	* Since each comparison is eliminating half the search space, this algorithm has a logarithmic property
		* And this algorithm performs in **O(log n)** time

## Let's do some TDD and write tests before we write the function!

```
def test_binarySearchNormal():
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 5) == True
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], -1) == False
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 11) == False
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 1) == True
	assert binarySearch([1,2,3,4,5,6,7,8,9,10], 10) == True

def test_binarSearchDuplicates():
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 0) == True
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 2) == False
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 4) == True
	assert binarySearch([-10,-5,0,1,1,4,4,7,8], 2) == False

def test_binarySearchEmptyList():
	assert binarySearch([], 0) == False

def test_binarySearchSameValues():
	assert binarySearch([5,5,5,5,5,5,5,5,5,5,5], 5) == True
	assert binarySearch([5,5,5,5,5,5,5,5,5,5,5], 0) == False
```

## Binary Search Implementation

```
def binarySearch(intList, item):
	first = 0
	last = len(intList) - 1
	found = False

	while first <= last and not found:
		mid = (first + last) // 2
		if intList[mid] == item:
			found = True
		else:
			if item < intList[mid]:
				last = mid - 1
			else:
				first = mid + 1
	return found
```
