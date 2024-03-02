#dsa #algorithms #datastructures #problemsolutions 


This note comes mostly from the book "Grokking Algorithms" by Aditya Bhargava. Details of the implementation in JS and Python are from other sources/my interpretation of the pseudocode in the book.

## Binary search and search problems generally
A simple example of a real world operation that is essentially a person applying a search algorithm is looking for a given name in a phone book. If you are looking for the last name "Kalashnikov", you could start at the beginning of the book and flip through checking every entry until you find it, but you are much more likely to intuitively understand that the "K" entries will be near the middle of the book and flip straight to approximately the right place to start looking. This is an example of simple and intuitive improvemnt to the efficiency of the naive approach to searching.

Binary search is an implementation of another such improvement to the naive approach. <span style="color: cyan;">Binary search as an algorithm takes a **sorted** array and a target element as inputs, and returns the index of the target in the array, or null if it is not present.</span>

## How binary search improves simple searching
Say that an evil wizard is thinking of a number between 1 and 100, and you are tasked with guessing it in the fewest possible tries. With every guess you make the wizard will tell you if your guess is too high, too low, or correct. 

Suppose that you start by guessing `1, 2, 3, 4 ....` etc. You will be told by the wizard that your guess is too low until you hit the correct number. You are only eliminating one possibility with every guess that you make. <span style="color: cyan;">This is simple search, or perhaps better called stupid search.</span> Simple search is a bad approach, in this example, in the worst case, you will make 99 guesses before hitting the correct number.

Binary search is based on a very simple improvement to this algo. By first guessing `50`, and being told too low by the wizard, you have eliminated half of the possible answers. Now you know that the answer is between 50 and 100, so you can take the midpoint of this new range, 75 and again eliminate half of the possibilities, repeating this until you arrive at the correct answer.

This is binary search , and in the case of guessing a number between 1 and 100, you will always arrive at the correct number in **at most, seven steps**, a huge improvement over the worst case of simple search.

<span style="color: cyan; font-weight: bold;">For any list of size _n_ simple search will take n steps in the worst case, where binary search will take log2 n steps in the worst case.</span>

## Implementation in python
Given the sorted array to search and the target element to find, the first thing to do is keep track of which part of the array you still need to search in. At first this will be the entire array, so you mark the search range with a high and low pointer at the 0 and -1 indexes of the array:
```python
low = 0
high = len(array) - 1
```

On each iteration of the search loop, you check the middle element of your low to high range against the target element:
```python
mid = (low + high) // 2
guess = array[mid]
```

Then if the guess is too low, you update the low pointer to mid plus one, and vice versa if the guess is too high.

Here is the full implementation:
```python
def binary_search(array, target):
	low = 0
	high = len(array) - 1

	while(low < high):
		mid = (low + high) // 2
		guess = array[mid]
		if guess == target:
			return mid
		if guess > target:
			high = mid - 1
		else:
			low = mid + 1
	return None
```

## Bin search running time
Whenever we talk about an algorithms efficiency or want to compare two algoritms, we will compare their running time. Simple search runs in linear time O(n), that is in the worst case you will need to check n items for a list of n length. <span style="color: cyan; font-weight: bold;">Binary search runs in logarithmic time O(log2n)</span>

