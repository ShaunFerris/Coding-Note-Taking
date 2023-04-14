#javascript #ES6 #functions 

There are alot of similarities in form and function between this topic and [[JavaScript - Object Destructuring]].

## The rest function
The rest function, denoted as `...args` works in a similar manner to `*args` in python, allowing a function to take multiple values as argument. When you use the rest function, args are passed as an array that can then be accessed in the function. It also allows us to use the map() filter() or reduce() functions to operate on the args without manually checking the array.

## Functions that work well on arg arrays
The functions talked about here are also discussed in [[JavaScript - Higher Order Functions and Functions as First Class Objects]].

Below will follow a short explanation of the map filter and reduce functions, which are all methods to be called on arrays and work great when used inside a function written to take multiple args with the rest function.

### map()
The map function is a method called on arrays that takes a callback function as it's arg, and returns a new array where each element is the result of the callback function being called on the corresponding element of the original array. For example:
```js
const array1 = [1, 4, 9, 16];

// Pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// Expected output: Array [2, 8, 18, 32]
```

### filter()
The **`filter()`** method creates a [shallow copy](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy) of a portion of a given array, filtered down to just the elements from the given array that pass the test implemented by the provided function.
```js
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// Expected output: Array ["exuberant", "destruction", "present"]
```

### reduce()
The **`reduce()`** method executes a user-supplied "reducer" callback function on each element of the array, in order, passing in the return value from the calculation on the preceding element. The final result of running the reducer across all elements of the array is a single value.

The first time that the callback is run there is no "return value of the previous calculation". If supplied, an initial value may be used in its place. Otherwise the array element at index 0 is used as the initial value and iteration starts from the next element (index 1 instead of index 0).

Perhaps the easiest-to-understand case for `reduce()` is to return the sum of all the elements in an array:
```js
const array1 = [1, 2, 3, 4];

// 0 + 1 + 2 + 3 + 4
const initialValue = 0;
const sumWithInitial = array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  initialValue
);

console.log(sumWithInitial);
// Expected output: 10
```

## The spread operator
The spread operator is similar to the rest function, and denoted by `...array`. It is used when to unpack an array into comma seperated values, and is useful when you want to for example provide values from an array to a function that cannot take an array as arg but can take many values:
```js
const myArray = [6, 89, 3, 45]
const maximus = Math.max(...myArray);
```

The spread operator is also discussed in [[Data Structures - Using Arrays]] along with it's python counterpart the splat operator.