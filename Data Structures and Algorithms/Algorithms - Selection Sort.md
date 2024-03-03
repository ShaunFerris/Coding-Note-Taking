#dsa #algorithms #datastructures #problemsolutions 

This note comes mostly from the book "Grokking Algorithms" by Aditya Bhargava. Details of the implementation in JS and Python are from other sources/my interpretation of the pseudocode in the book.

To follow this note, an understanding of the advantages of linked lists vs arrays is necessary, as well as big O notation. See: [[Algorithms - Big O Notation]], [[Data Structures - Linked Lists]], [[Data Structures - Memory and How Arrays Work]]

## A basic search application
Using the example from Grokking algorithms, say that you have a list of musical artists that you like on your computer, and for each one you have an associated play count. You want to sort this list by play count so that it is always easy to see who your favourite artist is at any given time.

The simple approach to this problem would be to go through the list and find the most played artist, then move it to a new empty list, removing it from the original. You would then repeat this process until the original list is empty and the new one is sorted. **This is selection sort**.

To find the highest playcount in the list, you would have to touch every element in the list. This is a linear time operation with time complexity O(n). However you have to check the highest element in the list one time for every element to complete the algorithm, so you are doing an O(n) operation n times.

<span style="color: cyan; font-weight: bold;">This means that the selection sort approach involves touching every element in the list to be sorte once for every element added to the output list, meaning the algorithm has O(n) * O(n) time complexity or O(n^2).</span>

## An aside on run times an big O
You might correctly point out at this point that the number of items you are checking gets smaller on each iteration of the loop, and this is true. You are not actually checking n items n times, but rather you check n, then n-1 then n-2 and so on. On average you really check a list that has 1/2n entries. This would give us a runtime of O(n * 1/2 * n),  but when we use big O notation for runtime, we discard constants, which 1/2 is, leaving us with O(n^2).

## Implementation
Here is an implementation of selection sort to arrange an array of integers from smallest to largest. It implements a helper function that is used for scanning the input array to find the smallest element.
```python
def find_smallest(arr):
	#keep track of the smallest element and it's index
	#start with the first element set to smallest and loop throught the rest
	smallest = arr[0]
	smallest_idx = 0
	for i in range(1, len(arr)):
		if smallest > arr[i]:
			smallest = arr[i]
			smallest_idx = i
	return smallest_idx

def selection_sort(arr):
	output = []
	for i in range(len(arr)):
		smallest_idx = find_smallest(arr)
		output.append(arr.pop(smallest_idx))
	return output
```

The above implementation is from the book Grokking Algorithms mentioned above, but I have also played with the below implementation that uses the built-in min method provided by python, to get the index of the minimum element in the start array. Benchmarking of this approach vs the above textbook one is something I haven't done yet but do plan to.
```python
from typing import List
import random

# implement selection sort using the inbuilt python min function
def select_sort(arr: List):
    #use values.index(min(values))
    output = []
    for i in range(len(arr)):
        small_idx = arr.index(min(arr))
        output.append(arr.pop(small_idx))
    return output

test_arr = random.choices(range(1, 10000), k=15)
print(select_sort(test_arr))
```