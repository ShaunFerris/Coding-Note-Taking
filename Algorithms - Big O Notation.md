#dsa #algorithms #algorithmoptimization 

The three factors that characterize an algorithms performance are:
1. Worst case: Use an input that gives the slowest perormance
2. Best case: Use an input that gives the best performance
3. Average case: Assume the input is random.

Big O deals with the worst case, as this is generally the one that gives us the most insight into the relative performance overall of the algorithm.

If we say that an algorithm has time complexity of O(n^2), that is essentially the same as saying that it's time to compute will grow proportionally with the square of the size of the input list. Similarly an algo with O(n) will have it's compute time grow linearly with the input size.

An algorithms time complexity can be catagorized generally speaking in these categories, <span style="color: cyan;">from most to least desirable</span>:
- O(1): <span style="color: cyan;">Constant time</span> 
- O(log n): <span style="color: cyan;">Logarithmic time</span>
- O(n):<span style="color: cyan;">Linear time</span>
- O(n log n):<span style="color: cyan;">Linearithmic time</span>
- O(n^2):<span style="color: cyan;">Quadratic time</span>
- O(2^n):<span style="color: cyan;">Exponential time</span>

## Determining the big O of an algorithm
Big O notation is a mathematically defined and derived system, but here we will not go over it in such depth, instead we will cover the important parts for programmers to understand and look at how to determine it from **inspection of an algorithm**.

Suppose you have a function, _f(a)_ with complexity of O(n^2) and it is executed _n_ times in a for loop:
```python
for i in range(n):
	f(a)
```
The loop operation is a linear time operation, it's time increases in proportion to how many times it is executed. This means that the total time complexity of this loop with our function in it is O(n^3),  determined by O(n^2) * O(n).

Another example would be nested loops:
```python
for i in range(n):
	for j in range(n):
		pass
```
Here the complexity is O(n) * O(n) = O(n^2). In the instance where the second loop iterates _m_ times instead of _n_, the complexity would now be O(nm).

