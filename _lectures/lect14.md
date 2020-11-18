---
num: "Lecture 14"
desc: "Mergesort, Quicksort"
ready: true
lecture_date: 2020-11-17 17:00:00.00-7:00
---

Recorded Lecture: [11_17_20](https://drive.google.com/file/d/1PQ77cLn8s3ECBY9AureTPADjJhL88o5e/view?usp=sharing)

# Divide and Conquer Algorithms

* Subdivide a larger problem into smaller problems
* Solve each smaller part
* Combine solutions of smaller sub problems back into the larger problem
* We’ve seen this pattern with recursion where the problem can be subdivided
* So far, we’ve talked about bubble sort, selection sort, and insertion sort
	* These algorithms generally run in O(n<sup>2</sup>)
* But better sorting algorithms exist!
	* We can improve our run time to **O(n log n)**

# Merge Sort

* **Idea:**
* Break a list into sub sublists where size == 1
	* A sublist with 1 element is considered sorted
* Merge each small sublist together to form sorted larger list
* Continue to merge sublists back into the original list

## Hand drawn example

![mergesortExample.png](mergesortExample.png)

## Merge Sort Implementation

```
def mergeSort(alist):
	if len(alist) > 1:
		mid = len(alist) // 2

		# uses additional space to create the left / right
		# halves
		lefthalf = alist[:mid]
		righthalf = alist[mid:]

		# recursively sort the lefthalf, then righthalf
		mergeSort(lefthalf)
		mergeSort(righthalf)

		# merge two sorted sublists (left / right halves)
		# into the original list (alist)
		i = 0 # index for lefthalf sublist
		j = 0 # index for righthalf sublist
		k = 0 # index for alist

		while i < len(lefthalf) and j < len(righthalf):
			if lefthalf[i] <= righthalf[j]:
			alist[k] = lefthalf[i]
			i = i + 1
			else:
			alist[k] = righthalf[j]
			j = j + 1
			k = k + 1

		# put the remaining lefthalf elements (if any) into
		# alist
		while i < len(lefthalf):
			alist[k] = lefthalf[i]
			i = i + 1
			k = k + 1

		# put the remaining righthalf elements (if any) into
		# alist
		while j < len(righthalf):
			alist[k] = righthalf[j]
			j = j + 1
			k = k + 1
```
```
# pytest
def test_mergeSort():
   numList1 = [9,8,7,6,5,4,3,2,1]
   numList2 = [1,2,3,4,5,6,7,8,9]
   numList3 = []
   numList4 = [1,9,2,8,3,7,4,6,5]
   numList5 = [5,4,6,3,7,2,8,1,9]
   mergeSort(numList1)
   mergeSort(numList2)
   mergeSort(numList3)
   mergeSort(numList4)
   mergeSort(numList5)

   assert numList1 == [1,2,3,4,5,6,7,8,9]
   assert numList2 == [1,2,3,4,5,6,7,8,9]
   assert numList3 == []
   assert numList4 == [1,2,3,4,5,6,7,8,9]
   assert numList5 == [1,2,3,4,5,6,7,8,9]
```

# Merge Sort Analysis

![mergesortAnalysis.png](mergesortAnalysis.png)

* Merge Sort breaks the list into two equal parts per recursive iteration
* Note that the height (number of levels) of the tree is **log n** (for 8 nodes, we have a height of 3).
* For each level, the merge step needs to compare `n` elements
	* So if there are **log n** levels and we need to do `n` comparisons to merge the elements for each level, then we have: **O(n log n)** running time

# Quicksort
* Another divide-and-conquer algorithm
* Can improve running times to **O(n log n)** in a typical case, but we’ll see how this can also lead to O(n<sup>2</sup>) in a worst-case scenario
* **Idea:**
* We can sort a list by subdividing the list based on a PIVOT value
	* Place elements < pivot to the left-side of the list
	* Place elements > pivot right-side of the list
	* Recurse for each left / right portion of the list
	* When sub list sizes == 1, then entire list is sorted

## How do we partition the list into left / right sub lists?
1. Choose pivot (typically first element in list)
2. leftmark index is on left-most side of list, rightmark index is on right-most side of list, and both leftmark and rightmark work inwards
3. Find an element in the left side (using leftmark index) that’s out-of-place (> pivot)
4. Find an element in the right side (using rightmark index) that’s out-of-place (< pivot)
5. Swap out-of-place elements with each other
	* We’re putting them in the correct side of the list!
6. Continue doing steps 1 - 5 until the rightmark index < leftmark index
7. Swap the pivot with rightmark index
	* We’re putting the pivot element in the correct place!
