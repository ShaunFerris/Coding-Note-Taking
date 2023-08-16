#interviewprep 

To start populating this note with good summaries of common JS focussed interview questions, I intend to summarise [this](https://www.youtube.com/watch?v=AHbAAnt9qsY&list=PLlrrMbxpJkWyAaYYbPsgrebF94E0Ndwq9&index=3&t=369s) video.

## Hoisting
JS lets you call a function before defining it. Function definitions are "hoisted" to the top of the code. The below will run in JS without throwing an error:
```javascript
doTheThing();

function doTheThing() {
	...
}
```

## Closure
Closure is a way to create a function that returns a function, so that you can scope some private variable to within the function. For example:
```javascript
function makeCounter() {
	let count = 0;

	return function {
		count++;
		return count;
	}
}

const myCounter = makeCounter();
myCounter();
myCounter();
myCounter();
myCounter();
myCounter();
console.log(myCounter());

//Console output: 5
```
What is happening here is that `makeCounter()` is a higer order function, and inside it we have declared some state (the count variable). Because of the way that scoping works in JS, the returned function has access to the scope above it, so it will have access to the state of the higher order function (the count).

We have created a closure, because the internal function has access to the count, but other code outside the higher order `makeCounter()` function do not. This is kinda like an internal private variable. For a more fine grained look at the concept see the [mozilla docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures).

