#datastructures #arrays #lists #javascript #python #mutabletypes

For a more technical look at how arrays work in memory and a good lead in to [[Data Structures - Linked Lists]], see [[Data Structures - Memory and How Arrays Work]].

These notes cover some fundamentals of JS arrays and python lists in a mostly language agnostic fashion. The terms list and array will be used interchangably. When language specific instances occur I will try to list the relevant methods in both languages.

## Basics
Lists in python are the same as arrays in JS. In both languages these datastructures are **mutable**, meaning you can alter a given element in place without needing to assign the result to a new variable or reference. 

Lists are sequential collections of other data. Each element in a list can be any standard data type or data structure. **Lists are mutable, but have a fixed order.** However, going back to the mutability of arrays, remember that they **do not** have a fixed length. Call an arrays length at any time with `myArray.length` in JS or `len(my_array)` in python.

## Indexing
Elements in a list are indexed in sequential numerical order starting from 0. An element at a given index in a list, say 5, can be referenced or operated on directly with the syntax `my_list[5] = 3`.

### Python specific - Advanced indexing with slices
In python, you can do more advanced slicing of lists with the general syntax `my_list[start : end : step]` where the start is the index to start the slice, the end is where to stop **non-inclusively** and the step is how many to jump by. The step is 1 by default, but it can be useful if you want to look at every second value, or to go backwards through the list by settin step to -1.

### JS specific - Use the slice method
Rather than modifying an array, `slice()` copies or _extracts_ a given number of elements to a new array, leaving the array it is called upon untouched. `slice()` takes only 2 parameters — the first is the index at which to begin extraction, and the second is the index at which to stop extraction (extraction will occur up to, but not including the element at this index). Consider this:
```js
let weatherConditions = ['rain', 'snow', 'sleet', 'hail', 'clear'];

let todaysWeather = weatherConditions.slice(1, 3);
```
`todaysWeather` would have the value `['snow', 'sleet']`, while `weatherConditions` would still have `['rain', 'snow', 'sleet', 'hail', 'clear']`.

## Methods to add and remove data
### In python
Add a value to the end of a list with the `append()` method. Add a value to the start of the list with `my_list.insert(0, value)`. Add a value to any index in a list with `my_list.insert(index, value)`.

Remove an item from the end with `my_array.pop()`, remove an item from any index with `my_array.pop(index)`. Remove an item by it's identity instead of it's index with `my_array.remove(value)`.

### In JS
Add a value to the end of an array with `myArray.push()`. Add to the start of the array with `myArray.unshift()`.

Remove a value from the end with `myArray.pop()`, remove from the start with `myArray.shift()`.

Add or remove an element from any position with the splice method:
Remove 1 element at index 3:
```js
const myFish = ["angel", "clown", "drum", "mandarin", "sturgeon"];
const removed = myFish.splice(3, 1);

// myFish is ["angel", "clown", "drum", "sturgeon"]
// removed is ["mandarin"]
```
Remove 0 elements before index 2 and insert "drum":
```js
const myFish = ["angel", "clown", "mandarin", "sturgeon"];
const removed = myFish.splice(2, 0, "drum");

// myFish is ["angel", "clown", "drum", "mandarin", "sturgeon"]
// removed is [], no elements removed
```

## Modifying arrays with the spread and splat operators
### JS
Copy an array by unrolling it's contents into a new array with the spread operator like this:
```js
let myArray = [1, 2, 3, 4, 5];
let myCopy = [...myArray]
```
Combine arrays in a similar way like this:
```js
let thisArray = ['sage', 'rosemary', 'parsley', 'thyme'];
let thatArray = ['basil', 'cilantro', ...thisArray, 'coriander'];
```
`thatArray` would have the value `['basil', 'cilantro', 'sage', 'rosemary', 'parsley', 'thyme', 'coriander']`.

### Python
In python you can do similar using the splat operator `*`:
```python
a = [1,2,3,4]
b = [10]
b.extend(a)
# b is [10, 1, 2, 3, 4]
```
 To create a copy of an array in python, use the copy or deepcopy method from the copy module.

## Sorting arrays
### JS
[For more detail on the js sort method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

In JS you use the sort method to sort an array. It will sort the list in place, rather making a copy or deepcopy. To sort into a new array object, copy it before sorting like this:
```js
const numbers = [3, 1, 4, 1, 5];
// [...numbers] creates a shallow copy, so sort() does not mutate the original
const sorted = [...numbers].sort((a, b) => a - b);
sorted[0] = 10;
console.log(numbers[0]); // 3
```
The **`sort()`** method sorts the elements of an array _[in place](https://en.wikipedia.org/wiki/In-place_algorithm)_ and returns the reference to the same array, now sorted. The default sort order is ascending, built upon converting the elements into strings, then comparing their sequences of UTF-16 code units values. **This means it will not sort in numerical order by default.**

The syntax of the sort method is:
```js
sort()
//or
sort(compareFn)
```
The optional compareFn argument is a function that defines the sort order. It is written as an anonymous function using `a` and `b` as the args: [[General - Annonymous Functions]].
The return value should be a number whose positivity indicates the relative order of the two elements. The function is called with the following arguments, `a` and `b`, the first and second elements to compare.

If omitted, the array elements are converted to strings, then sorted according to each character's Unicode code point value.

If `compareFn` is supplied, all non-`undefined` array elements are sorted according to the return value of the compare function (all `undefined` elements are sorted to the end of the array, with no call to `compareFn`).

<figure class="table-container">
  <table border="1">
    <thead>
      <tr>
        <th><code>compareFn(a, b)</code> return value</th>
        <th>sort order</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>&gt; 0</td>
        <td>sort <code>a</code> after <code>b</code>, e.g. <code>[b, a]</code></td>
      </tr>
      <tr>
        <td>&lt; 0</td>
        <td>sort <code>a</code> before <code>b</code>, e.g. <code>[a, b]</code></td>
      </tr>
      <tr>
        <td>=== 0</td>
        <td>keep original order of <code>a</code> and <code>b</code></td>
      </tr>
    </tbody>
  </table>
</figure>

So the required statement with a return function to sort an array of numbers in ascending order is:
```js
arr.sort((a, b) => a - b);
```

### Python
In python the default mode of the sort method will sort a numerical list in ascending order.

The syntax of the `sort()` method is:
```python
list.sort(key=..., reverse=...)
```
Alternatively, you can also use Python's built-in [sorted()](https://www.programiz.com/python-programming/methods/built-in/sorted) function for the same purpose.
```python
sorted(list, key=..., reverse=...)
```
By default, `sort()` doesn't require any extra parameters. However, it has two optional parameters:

-   **reverse** - If `True`, the sorted list is reversed (or sorted in Descending order)
-   **key** - function that serves as a key for the sort comparison

The `sort()` method doesn't return any value. Rather, it changes the original list.
If you want a function to return the sorted list rather than change the original list, use `sorted()`.

If you want your own implementation for sorting, the `sort()` method also accepts a `key` function as an optional parameter.

Based on the results of the key function, you can sort the given list.
```python
list.sort(key=len)
```
In the above case the function to sort by is the built in `len()` function, and the array will be sorted by element length. You can also use functions that you have defined or lambda functions here.

## Higher order array functions
### Javascript
Notes on higher order js array functions are here: [[JavaScript - Higher Order Functions and Functions as First Class Objects]].

### Python
The higher order functions discussed in the linked js notes above, in particular map, filter and reduce, also exist in python [[Python - Map, Filter and Reduce Functions]]