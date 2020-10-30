---
num: "Lecture 9"
desc: "Recursion"
ready: true
lecture_date: 2020-10-29 17:00:00.00-7:00
---

Recorded Lecture: [10_29_20](https://drive.google.com/file/d/1EV1NOgcjfxmUJ0xl1O5xvmN2hsxhf4v9/view?usp=sharing)

# Recursion

* Recursion is when a function contains a call to itself
* Recursive solutions are useful when the result is dependent on the result of sub-parts of the problem

## The three laws of recursion

1. A recursive algorithm must have a base case
2. A recursive algorithm must change its state and move toward the base case
3. A recursive algorithm must call itself, recursively

## Common Example: Factorial

* N! = N * (N-1) * (N-2) * ... * 3 * 2 * 1
* N! = N * (N-1)!
* We can solve N! by solving the (N-1)! sub-part
	* Note: 0! == 1

```
def factorial(n):
	if n == 0:      # base case
		return 1
	return n * factorial(n-1)
```

## Function Calls and the Stack

* The `Stack` data structure has the First in, Last out (FILO) property
* We can only access the elements at the top of the stack
	* Analogy: Canister of tennis balls
		* In order to remove the bottom ball, we have to remove all the balls on top 
* We won't go through an implementation yet, but we can conceptualize this on a high-level
* Function calls utilize a stack ("call stack") and organize how functions are called and how expressions that call functions wait for the functions' return values
	* When a function is called, you can think of that function state being added (or "pushed") on the stack
	* When a function finishes execution, it gets removed (or "popped") from the stack
	* The top of the stack is the function that is currently running
* Example

```
def double(n):
	return 2 * n

def triple(n):
	return n + double(n)

print(triple(5))
```

![triple.png](triple.png)

* Going back to our recursive factorial example, let's trace `factorial(3)`

![factorial.png](factorial.png)

## Common Example: Fibonnaci Sequence

* A fibonacci sequence is the nth number in the sequence is the sum of the previous two (i.e. f(n) = f(n-1) + f(n-2)).
* f(0) = 0, f(1) = 1, f(2) = 1, f(3) = 2, f(4) = 3, f(5) = 5, f(6) = 8, ...

```
def fibonnaci(n):
	if n == 1:    # base cases
		return 1
	if n == 0:          
		return 0

	return fibonnaci(n-1) + fibonnaci(n-2)
```

* Evaluate `fibonnaci(4)` by hand

```
fib(3)                    +  fib(2)
fib(2)          + fib(1)  +  fib(2)
fib(1) + fib(0) + fib(1)  +  fib(2)
1      + fib(0) + fib(1)  +  fib(2)
1      + 0      + fib(1)  +  fib(2)
1      + 0      + 1       +  fib(2)
1      + 0      + 1       +  fib(1) + fib(0)
1      + 0      + 1       +  1      + fib(0)
1      + 0      + 1       +  1      + 0
3
```

## Recursive Binary Search

```
def binarySearch(intList, item):
	if len(intList) == 0:
		return False
	else:
		mid = len(intList) // 2
		if intList[mid] == item:
			return True
		elif item < intList[mid]:
			return binarySearch(intList[:mid], item)
		else:
			return binarySearch(intList[mid+1:], item)
```
