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

Here is a useful diagram from the book "Grokking Algorithms" that illustrates some common run times:
![[Pasted image 20240304140350.png]]
The actual numbers here are not important, they are based on a theoretical slow ass computer that can only execute ten instructions per second, but just look at the relationship between time and array size.

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

Now, consider the previous example, but with print statements inside the nested loops:
```python
for i in range(n):
	for j in range(n):
		print(i)
		print(j)
```
The print statements have complexity of O(1), because no matter how big the argument given to the print function, it performs the same number of operations each time. Since the print functions are sequential, the complexity of the inner loop is O(1) + O(1) = O(2), **however**, as mentioned earlier, we must supress or ignore constants. Since 2 is a constant we consider the compleity of the inner loop body to just be O(1). To clarify, <span style="color: cyan;">regardless of how large the variables i and j become, the inner loop will still complete the same number of operations, so we consider it constant time.</span> Thus the complexity of the whole code block including the loops is still O(n^2).

### Logarithmic complexities
To simply understand what a logarithmic complexity means, <span style="color: cyan;">O(logn) means that compute time will increase linearly as the input size increases exponentially.</span> For example, if an algo with this time complexity takes 1 second to complete with an input size of 10, then it will take 2 seconds with an input of 100.

This complexity is typically seen for divide and conquer type algorithms like binary search and quicksort where the input size is halved on each iteration of a loop. In the case of quicksort, which will be covered in more detail in a future note, we divide the data into two parts each iteration, and sort those two parts. The action of splitting the data is an O(n) operation, which gives us a total complexity of O(n log n) = O(n) * O(log n).

## Time complexity desireability
Generally speaking we can divide possible algorithm runtime complexities into good and bad categories. The good or desireable category contains:
- O(1)
- O(log n)
- O(n)
- O(n log n)
While the so called bad news category of complexities to try and avoid is:
- O(n^k) where k is >= 2
- O(2^n)
- O(n!)

As an example, if we need to sort a list and we have a choice between two algorithms to use
whose algorithmic complexities are O(n log n) and O(n^2) respectively, we would want to use
the algorithm whose runtime complexity is O(n log n) as we know that as the input size
increases that this algorithm is going far out perform the algorithm with a runtime complexity
of O(n^2). This is shown in the graph above.