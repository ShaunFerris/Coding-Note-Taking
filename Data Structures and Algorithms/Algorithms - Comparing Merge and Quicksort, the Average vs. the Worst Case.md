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
