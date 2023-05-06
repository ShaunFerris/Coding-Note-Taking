#javascript #python #highlevelconcepts #general

**Notes on this topic are adapted  from the content in freeCodeCamps JavaScript algorithms and data structures course.**

A recursive function in computer science, is one that is expressed in terms of itself ie: a function that contains in it's code block, a function call to itself.

More broadly, recursion is a method of solving problems where the solution depends on solutions to smaller instances of the same problem.

Imagine having a task requiring you to **multiply the first n elements of an array and return the final product**. You could use a for loop as in this example:
```js
// Approach A - Looping
function multiply(arr, n) {
	let product = 1;
	for (let i = 0; i < n; i++:) {
		product *= arr[i];
	}
	return product;
}
```
Or the problem can be solved recursively:
```js
// Approach B - Recursion
function multiply(arr, n) {
	if (n <= 0) {
		return 1;
	} else {
		return multiply(arr, n-1) * arr[n-1];
	}
}
```

The recursive solution works because `multiply(arr, n)` is equivalent to:
`mutliply(arr, n-1) *  arr[n - 1]` 
Ie: if you multiply together the first 5 elements of the array `[1, 2, 3, 4, 5]`, you get the same result as if you multiply the first 4 elements together, and then multiply again by array indexed at 4, aka the fifth number.

The recursive version breaks down like this:
1. In the **base case**, (n <= 0), it returns 1.
2. If n > 0 it calls itself again with n - 1. this means it will always get to calling multiply(arr, 1).
3. When the reursive function calls itself with n = 1, it will return this result into the waiting fucntion call from when it was called with n = 2, and then multiply it by `arr[n -1]` and return that into the function call from the n = 3 call and so on.

**Recursive functions must have a base case where they return something without calling themselves.**

