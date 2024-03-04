#dsa #algorithms #datastructures #algoTechniques

This note is working off the book Grokking Algorithms.

## Divide and conquer and the concept of algorithm strategies
Sometimes you will come across a problem that can't be solved by any of the algorithms that you have learnt and memorised. This is a situation that will come up all of the time, because it is simply not reasonable nor efficient for every engineer to memorise every algorithm for every task.

<span style="color: cyan; font-weight: bold">A good engineer should have a toolbox of techniques they have seen applied to algorithmic problems that they can apply to new probelems, the Divide and Conquer strategy is one such technique.</span>

The sliding window technique is another such generalised technique that can be applied to multiple algo problems, see here: [[Algorithms - Sliding Window Algorithms]].

This note will go over the DaC strategy generally, outlining how it works and what kind of problems it can be applied to. For a named algorithm that uses the recursive DaC strategy, see the note on quick sort here: [[Algorithms - Quicksort]].
## How to divide and conquer
Divide and Conquer (here on out DaC) is a generalised technique for algorithms, and is by nature **recursive**.

We will go through a couple of different examples to explain the strategy, and the strategy for working with recursive approaches generally, and after that you should follow the above link to the quicksort note for a third example.

The basic outline for how to apply DaC to a problem is very similar to the basic strategy for applying recursion: 
1. Figure out a simple case to use as a base case for the problem
2. Figure out how to systematically reduce the problem over and over in a way that converges to the base case.

### The land division example
This example comes straight from the Grokking Algos book, and is fairly complicated. It also relies on the Euclidean algorithm proof, which is too complicated to be an aside in this note, but may be covered in a future note: [[Algorithms - The Euclidean Algorithm]].

**Suppose you are a farmer with a plot of land that you wish to divide into equally sized smaller subplots that must all be square. You want every square subplot to be the same size but also as large as possible**.
![[Pasted image 20240303104114.png]]![[Pasted image 20240303104129.png]]
To apply DaC to this problem we first need to figure out the base case, and then find  way to reduce the problem to the base case in steps.

A good simple base case for this problem would be if one side was a clean multiple of the other side:
![[Pasted image 20240303104318.png]]
50 divides cleanly by 25 to give 2, which means you can make two equally sized squares that are 25 by 25.

If this is the base case, we next need to figure out a recursive case. Each call to the recursive algorithm should reduce the size of the problem toward the base case. <span style="color: cyan; font-weight: bold;">For this problem we can achieve this by marking out the two biggest boxes we can fit into the original problem space, and then applying the same algorithm to the remainder.</span>
![[Pasted image 20240303104747.png]]
This is where the proof for Euclids algorithm woudld be useful to understand. We can still proceed without it though so long as we accept as fact this conclusion: **That the largest square size that will work for the remainder 400 X 640m section is also the largest square that will work for the whole plot**.

If we continue to reduce the remainder by finding the largest square and recurring on the remainder, we will eventually hit the base case, where one side is a clean multiple of the other, and that will give us the largest square for the whole plot.
![[Pasted image 20240303105146.png]]
This algorithm can be implemented pretty simply in python, here is my first shot at it, which works but has not been given a wide array of test cases:
```python
def largest_common_square(side_a, side_b):
    large = max(side_a, side_b)
    small = min(side_a, side_b)
    print(large, small)
    # base case
    if large % small == 0:
        return small
    else:
        return largest_common_square(side_a=small, side_b=(large % small))


print(largest_common_square(1680, 640))
```

Now lets look at a simpler example to solidify the concept of recursion as applied to DaC algorithm strategies.

### The simple array sum example
Say that you are given an array of numbers and tasked with adding them all together to give the total sum (ignore for now the existence of the sum() function). This is intuitively very simple to implement with a loop:
```python
def arr_sum(arr: List):
	output = 0
	for i in arr:
		output += i
	return output
```
But this could also be done recursively with DaC. To do this we need to do what we always do with this strategy, first find a base case, and then find a recursive case that will reduce the size of the problem till it meets the base case.

For the base case, think of the simplest case we could see. An array containing 0 or one elements has a pre-defined sum, either 0 or the single element, so this can be the base case.

Now how do we reduce the problem step-wise toward this case? The base case is an array of one or zero elements, so we need to reduce the number of elements with each step. `sum(2, 3, 6)` is equivalent to `2 + sum(3, 6)`, so our recursive case could be removing one element from array and adding it to the recursive call, like this:
```python
from typing import List

def recursive_sum(arr: List):
    # base case
    if len(arr) == 0:
        return 0
    if len(arr) == 1:
        return arr[0]
    # recursive case
    else:
        return arr.pop() + recursive_sum(arr)


print(recursive_sum([1, 2, 3, 4, 5, 6, 7]))
```
 At this point we have a good understanding of how to apply recursion, which is really what the DaC strategy is, so now go and read up on quicksort.