#javascript #whileloops #forloops 

## While loops
While loops in JS work pretty much as you'd expect, and follow the same basic formatting rules as  if statements [[JavaScript - Basics]]:
```js
let i = 0;
while (i < 5) {
	i++;
	console.log(i);
}
```

## For loops
For loops in JS are written in a way that exposes their inner workings much more than pythons for loops. Their general structure is `for (a; b; c){}` where:
- a = the initialization statement. Executes only once at the start of the loop, usually used to set-up a variable that will be incremented with each loop.
- b = the condition statement. This is evaluated once every iteration of the loop and is usually used to define under which condition the loop will terminate.
- c = the final expression. Evaluated once every loop like the condition statement, but at the end of each loop iteration instead of the start, typically used to increment the variable.

### Example for loop - push  numbers 0 to 10 to an array
NOTE: the push method is the JS equivalent of pythons append.
```js
const ourArray = []
for (let i = 0; i <= 10; i++) {
	ourArray.push(i);
}
```

### for...of loops
You can also implement for loops in javascript that match up with the default way of doing for loops in python, where each iteration of the loop is an operation on each element of an iterable object in order. This is done with a for...of loop, eg:
```js
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}

// Expected output: "a"
// Expected output: "b"
// Expected output: "c"
```

### for...in loops
Similar to for...of loops, for...in loops iterate over iterable objects, but where for...of loops iterate over the values, for...in loops iterate over the keys. In objects (dicts) this is straightforward. In lists this means that the loop will iterate over the indexes of each entry.

### do...while loops
Do...while loops will always do one pass of the code in the loop, and then continue until the loop condition is no longer true.
In the following example, the array will contain 5 after code execution where in  a normal for loop it would be empty:
```js
const myArray = []
let i = 5;
do {
	myArray.push(i);
	i++;
	
} while (i < 5);
```

### Recursive solutions
For loops (iterative solutions) can often be replaced with recursive solutions. See [[General - Basic Recursion]].