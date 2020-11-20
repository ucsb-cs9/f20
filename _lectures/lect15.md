---
num: "Lecture 15"
desc: "Quicksort cont., Trees"
ready: true
lecture_date: 2020-11-19 17:00:00.00-7:00
---

Recorded Lecture: [11_19_20](https://drive.google.com/file/d/15b0ZP7N4HF4ZqQJ-xmnpo6YNHSxgfMqH/view?usp=sharing)

## Quick Sort Hand Drawn Example

![quicksortExample.png](quicksortExample.png)

## Quick Sort Implementation

```
def quickSort(alist):
	quickSortHelper(alist, 0, len(alist) - 1)

# helper function so we can pass in the first / last index
# of lists
def quickSortHelper(alist, first, last):
	if first < last:

		# will define the indices of the left / right sub lists
		splitpoint = partition(alist, first, last)

		# recursively sort the left / right sub lists
		quickSortHelper(alist, first, splitpoint-1) # left
		quickSortHelper(alist, splitpoint+1, last) # right

# partition function will organize left sublist < pivot
# and right sub list > pivot
def partition(alist, first, last):
	pivotvalue = alist[first] # choose first element as pivot

	leftmark = first + 1
	rightmark = last

	done = False
	while not done:

		# move leftmark until we find a left element > pivot
		while leftmark <= rightmark and alist[leftmark] <= pivotvalue:
			leftmark = leftmark + 1

		# move rightmark until we find a right element < pivot
		while rightmark >= leftmark and alist[rightmark] >= pivotvalue:
			rightmark = rightmark - 1

		# check if we're done swapping left / right elements in
		# correct side
		if rightmark < leftmark:
			done = True
		else: # swap left and right elements into correct side of list
			temp = alist[leftmark]
			alist[leftmark] = alist[rightmark]
			alist[rightmark] = temp

	# put the pivot value into the correct place (swap pivot w/ rightmark)
	temp = alist[first] # pivot
	alist[first] = alist[rightmark]
	alist[rightmark] = temp

	return rightmark
```

## Quick Sort Analysis

* Best-case running time is **O(n log n)**
	* In the best case, there are **log n** levels. Each level is **O(n)** when performing the partition step
* However, the worst case is **O(n<sup>2</sup>)**
	* Consider the case where the list is already sorted (or in reverse order)
	* The sub lists aren't evenly divided for every recursive call
	* Quick Sort performance is dependent on the pivot value!
	* Can try to improve the pivot choice by selecting random values and choosing the medium
	* Textbook describes the median of three approach
		* Choose first, middle, and last element. Choose the median of these values
		* But even then, there is no guarantee that these values are good pivot values, but it does improve our chances that they are
* Note that Quicksort DOES NOT need extra space (unlike merge sort)

![quicksortAnalysis.png](quicksortAnalysis.png)

# Trees

## Terminology

* Node - An element in the tree. May have an incoming edge and many outgoing edges.
* Edge - A connection between nodes (can be directional or bidirectional)
* Root - The top most node (node without any incoming edges)
* Path - The sequence of nodes from one node to a destination node along the tree
* Children - Nodes that have incoming edges from another node
* Parent - Contains outgoing edges to other child nodes
* Sibling - Nodes that have the same parent
* Subtree - A tree structure where the root of the tree is a child of a parent
* Leaf - A node without any outgoing edges
* Level - Number of edges from the root node to a destination node
* Height - Maximum level of the entire tree
