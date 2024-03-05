#dsa #algorithms #datastructures

## Mergesort vs. quicksort
Suppose that you have the following simple function for printing each item in a given array:
```python
def printer_func(arr: List):
	for item in arr:
		print(item)
```
Because this function touches every item in the list, it has a runtime with O(n). Now compare it to this version:
```python
from time import sleep 
def sleepy_print(arr: List):
	for item in arr:
		sleep(1)
		print(item)
```
Both functions loop through the list once, touching every item in the array, so they both have O(n) runtime, however obviously the sleepy_print function will take longer, because it pauses for one second on every iteration.

This is because when we write big O notation like `O(n)` what we are really talking about is `O(c*n)` where c is some constant representing a fixed amount of time that the algo takes. For example the constant might be around 10ms for `printer_func` and one second for `sleepy_print`. <span style="color: cyan; font-weight: bold;">You usually ignore constants when discussing runtimes because if two algos have different runtimes then the constant is neglible.</span>

For an example of this, think about simple search (stupid search) and binary search, with runtimes of O(n) and O(log n) respectively. In a scenario where the constant for simple search is 10ms and the constant for bin search is 1 full second, you might think that simple search would be faster, but if you compare the times for searching 4billion records, you see that bin search is still WAAAAAAYYYYy faster:
![[Pasted image 20240304171102.png]]
 So the constant usually does not matter, however sometimes it DOES, and the case of quicksort vs merge sort is one such case. <span style="color: cyan; font-weight: bold;">Quicksort has a much smaller constant than mergesort, so if both are running in O(n log n) time, it will be much faster, and it usually is because quicksort hits the average case way more often than not.</span>

So what actually are the average and worst cases for quicksort?

## Average vs. worst case - Pivot picking in quicksort
The performance of quicksort relies very heavily on the choice of pivot. In my initial implementation of the quicksort algo ([[Algorithms - Quicksort]]), we always just picked the first element of the input array to use as the pivot. Now imagine that this algo is passed an array that is already sorted. Because it does not do any checks for this case, it will still try to sort it. 
![[Pasted image 20240305121518.png]]
In this case, you aren't partitioning the list into halves, or even getting  close to halves, which ends up in a really long call-stack. In fact there is one call on the stack for every element in the array. This is slow as heck.

Now imagine that you take that same pre-sorted array and pick the middle element as the pivot.
![[Pasted image 20240305132541.png]]
Now you actually are partitioning into halves each time, and as a result the call stack is much shorter.

<span style="color: cyan; font-weight: bold;">The first example above is the worst case scenario, and the second one is the best case. They have runtimes of O(n) and O (n log n) respectively.</span>

Now look at the first step. You pick one element as the pivot, and divide all the other elements into partition arrays. This operation is O(n)elements as you are touching all 8 elements. **This operation is O(n) in both the best and worst case**.This is also the same on each level of the call stack. Every recursive call touches each element once to pick a pivot and then partition.
![[Pasted image 20240305133148.png]]
In the example where the pivot is in the middle, there are n log n levels, and each level takes O(n) time to complete, giving you O(n log n) in the best case. In the worst case there are n levels, hence O(n^2) in the worst case. <span style="color: cyan; font-weight: bold;">The important thing to remember though is that the best case is also the average case for quick sort. If you are always picking a random element as the pivot, then it will run in O(n log n) time on average.</span>

