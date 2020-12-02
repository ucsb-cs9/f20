---
layout: lab
num: lab08
ready: true
desc: "Go Fish!"
assigned: 2020-12-02 23:59:59.59-7
due: 2020-12-13 23:59:59.59-7
---

In this lab, you'll have the opportunity to practice:

* Defining classes in Python
* Overloading the `==`, `<`, and `>` operators in a Python class
* Implementing and applying the Binary Search Tree (BST) data structure with duplicates
* Writing functions that ensure the list of Card objects are in sorted order
* Testing your functionality with pytest

**Note:** It is important that you start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the week before the deadline if needed!

# Introduction

The goal for this lab is to write a program that will manage cards for a card game. All cards have a suit and a rank, which can be used to determine the value of cards in relation to each other. All cards will be managed by a Binary Search Tree (BST) where the best card is the maximum and the worst card is the minimum.

In order to manage cards for this lab, you will design a card class (`Card`) and a `PlayerHand` class that organizes the cards in a BST data structure.

You will also write pytests in `testFile.py` illustrating your behavior works correctly. This lab writeup will provide some test cases for clarity, but the Gradescope autograder will run different tests shown here.

# Instructions

You will need to create three files:
* `Card.py` - Defines a Card class. For simplicity, this class will assume all Cards have a `suit` and a `rank`
* `PlayerHand.py` - Defines a PlayerHand (Binary Search Tree) class that is an ordered collection of a Player's Cards. You can adapt the Binary Search Tree implementation shown in the textbook supporting the specifications in this lab.
* `testFile.py` - This file will contain your pytest functions that tests the overall correctness of your class definitions

There will be no starter code for this assignment, but rather class descriptions and required methods are defined in the specification below.

You should organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import / from` technique shown in lecture. 

# Card.py

The `Card.py` file will contain the definition of a Card class. The Card class will hold information about the cards (`suit` and `rank`), and for simplicity, it will also double as a node for our `PlayerHand`. We will define the Card attributes as follows:

* `suit` - data to distinguish what suit that card is: C (club), D (diamond), H (heart), or S (spade)
* `rank` - data to distinguish the rank of the card: A (ace), 2, 3, 4, 5, 6, 7, 8, 9, J (Jack), Q (Queen), K (King). Assume there is no Joker.
* `parent` - a reference to the parent node of a card in the Binary Search Tree, `None` if it has no parent (aka, it is the root)
* `left` - a reference to the left child of a card in the Binary Search Tree, `None` if it has no left child
* `right` - a reference to the right child of a card in the Binary Serach Tree, `None` if it has no right child
* `count` - a number representing the amount of times this card appears in the Binary Search Tree. `1` by default, but it can be greater.

You should write a constructor that allows the user to construct a Card object by passing in values for the suit and rank. Your constructor should also create the `count` attribute and initialize it to 0, and create the `parent`, `left`, and `right` attributes initialized to `None`.

* `__init__(self, suit, rank)`

Your Card class definition should also support the "getter" and "setter" methods.

* `getSuit(self)`
* `setSuit(self, suit)`
* `getRank(self)`
* `setRank(self, rank)`
* `getCount(self)`
* `setCount(self, count)`
* `getParent(self)`
* `setParent(self, parent)`
* `getLeft(self)`
* `setLeft(self, left)`
* `getRight(self)`
* `setRight(self, right)`

Lastly, your Card class *can* overload the `>`, `<`, and `==` operators. This is optional, but it can be helpful when inserting cards into their proper position within the `PlayerHand` Binary Search Tree. In this context, a `Card` should first be compared by its `rank`. For our purposes, we treat A (Ace) as the smallest, and K (King) as the largest. If the `rank` is equal, we then compare the `suit` of the cards, where C (Club) < D (Diamond) < H (Heart) < S (Spade). By this logic, `==` should only return True if both the `suit` and the `rank` are equal.

# PlayerHand.py

The `PlayerHand.py` file will contain the definition of a `PlayerHand` class. This will keep track of the cards a player has in their hand, implemented as a Binary Search Tree. The `PlayerHand` will manage `Card` objects based on their `suit` and `rank`. 

* `__init__(self)` - the constructor for the `PlayerHand` will simply initialize the empty Binary Search Tree.

In addition to the construction of the MinHeap in this class, these methods are required to be implemented:

* `getTotalCards()` - returns the total number of cards in hand
* `getMin()` - returns the card with the lowest value from the player's hand
* `getSuccessor(suit, rank)` - attempts to finds the Card with the `suit` and `rank`, and returns the card with the next greatest value. Returns `None` if there is no card with the specified `suit` and `rank`, or if the Card is the maximum and has no successor.
* `put(suit, rank)` - this adds a card with the specified `suit` and `rank` to the BST. If that Card already exists in the BST, increment the `count` for that Card.
* `delete(suit, rank)` - attempts to find the Card with the specified `suit` and `rank`, and decrements the Card `count`. If the count is `0` after decrementing the `count`, remove the node from the BST entirely. Returns `True` if the Card was successfully removed or decremented, and `False` if the card was not present in the BST.
* `isEmpty()` - returns `True` if there are no cards in the BST and returns `False` otherwise.
* `get(suit, rank)` - attempts to find the Card with the specified `suit` and `rank`, and returns the object if it exists. Otherwise, return `None`
* `inOrder()` - returns a string with the inOrder traversal of the BST. Printing the in-order traversal should help check that the cards are in the correct order in the tree.
* `preOrder()` - returns a string with the preOrder traversal of the BST. BSTs with the same structure should always have the same pre-order traversal, so this can be used to verify that everything was inserted correctly.

An example of the `inOrder()` string format is given below:
```python
hand = PlayerHand()
hand.put('D', 'A')
hand.put('S', 'K')
hand.put('S', '2')
hand.put('C', 'Q')
hand.put('H', '7')
hand.put('S', 'K')

assert hand.inOrder() == \
"C Q | 1\n\
D A | 1\n\
H 7 | 1\n\
S 2 | 1\n\
S K | 2\n"
```

An example of the `preOrder()` string format is given below:
```python
hand = PlayerHand()
hand.put('D', 'A')
hand.put('S', 'K')
hand.put('S', '2')
hand.put('C', 'Q')
hand.put('H', '7')
hand.put('S', 'K')

assert hand.preOrder() == \
"D A | 1\n\
C Q | 1\n\
S K | 2\n\
S 2 | 1\n\
H 7 | 1\n"
```

Other than the required methods, feel free to implement any helper methods that you think are useful in your implementation. The automated tests will test only your implementation of the required methods by creating a `PlayerHand` containing various `Cards` with different `suit` and `rank` attributes. The `delete()` and `put()` methods will be run, with `get()`, `inOrder()`, and `preOrder()` being used to verify that the `PlayerHand` is fully functional. You should write similar tests to confirm the BST is working properly.

# testFile.py

This file should test all of your classes using pytest. Think of various scenarios and edge cases when testing your code according to the given descriptions. You should test every class' method functionality (except for getters / setters). Even though Gradescope will not use this file when running automated tests (there are separate tests defined for this), it is important to provide this file with various test cases (testing is important!!).

Of course, feel free to reach out / post questions on Piazza as they come up!

# Submission

Once you're done with writing your class definitions and tests, submit the following files to Gradescope's Lab08 assignment:

* `Card.py` 
* `PlayerHand.py`
* `testFile.py`

There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

If the tests don't pass, you may get some error message that may or may not be obvious at this point. Don't worry - if the tests didn't pass, take a minute to think about what may have caused the error. If your tests didn't pass and you're still not sure why you're getting the error, feel free to ask your TAs or Learning Assistants.