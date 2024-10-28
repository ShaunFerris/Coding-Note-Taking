#dsa #algorithms #datastructures #problemsolutions 

Like a lot of my notes on named algos, this note follows the book Grokking Algorithms.

Quicksort is a sorting algorithm. While I write this note I have so far only covered selection sort, which is essentially the naive approach to sorting an array. Unlike selection sort, quicksort is much faster, and is actually used commonly in realworld applications, for example it is included in the C standard lib. <span style="color: cyan; font-weight: bold;">Quciksort uses the divide and conquer strategy and is thus a recursive algo.</span>

## Implementation
Just like the simple array summing example in my notes on the DaC strategy, quicksort will use the base case of an empty or single element array:
```python
def part_quicksort(arr: List):
	if len(arr) < 2:
		return arr
```
If we move up in complexity from the base case, the next case we see is that of an array with exactly **two** elements. In this case it is very simple to sort, we check if the first element is smaller than the second, if it is not, we switch them. This case can be thought of as essentially a second base case for the algorithm. 

Now lets increase the case complexity again and look at an array of three elements. Say we have this array `[33, 15, 10]`. To break this down towards the base case, quicksort has us pick a pivot. This can be any element from the array, and we will then look at sub arrays of elements that are smaller or larger than the pivot, **this process is called partitioning**. We will discuss later how to pick pivots optimally, but for now lets just use the first element. Our original array is now partitioned like this: 
![[Pasted image 20240304134332.png]]
The two sub-arrays are not sorted, but if they were, we could assemble the final sorted array by just appending the large array to the pivot and then appending the result to the small array. BUT! <span style="color: cyan; font-weight: bold;">The base cases we have covered already know how to sort arrays of length 0, 1 and 2, so we can apply these operations recursively and get to the final result!</span> This will work no matter which array element you chose to be the pivot.

Here is an implementation of quicksort:
```python
from typing import List
import random


def quicksort(arr: list):
    # base case
    if len(arr) < 2:
        return arr
    # recursive case - pivot and partitions
    else:
        pivot = arr[0]
        lessers = [i for i in arr[1:] if i <= pivot]
        greaters = [i for i in arr[1:] if i >= pivot]
        return quicksort(lessers) + [pivot] + quicksort(greaters)


test_arr = random.choices(range(1, 10000), k=15)
print(quicksort(test_arr))
```

## Quicksort pivot picking and runtime
Quicksort has a special quirk compared to other common algos, in that it's runtime is dependant on the pivot that is chosen for a given array. Before we get into the nitty gritty of why this is, lets quickly look at some common big O runtimes again:
![[Pasted image 20240304140527.png]]
The actual numbers here are not important, they are based on a theoretical slow ass computer that can only execute ten instructions per second, but just look at the relationship between time and array size.

The other sorting algo we have looked at while I am writing this is selection sort, it is very slow with runtime of O(n^2). Soon we will look at merge sort, which is much faster at O(n log n). The diagram above shows quicksort to have runtime O(n log n) as well, but this is only partially true. <span style="color: cyan; font-weight: bold;">In the worst case, quicksort has the same runtime as the slow as balls selection sort, O(n^2). BUT in the average case, it has O(n log n).</span>

So, if quicksort is only O(n log n) in the average case, but mergesort has that runtime always, why do we ever use quicksort? See this note for a quick overview of mergesort: [[Algorithms - Mergesort]]
And this note for a discussion of mergesort vs. quicksort and the average vs. worst cases: [[Algorithms - Comparing Merge and Quicksort, the Average vs. the Worst Case]]
