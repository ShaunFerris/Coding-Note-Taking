#interviewprep #revision

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

## var let and const
The three different ways to declare a variable in JS. `var` and `let` are for mutable variables, so variables declared this way can be reassigned later on, while `const` variables can not. `const` variables can be mutated, but not reassigned. Variables declared with `var` are globally scoped, so if they are declared within a block, such as a for loop, they are still accessible to code outside that block. Variables declared with `let` or `const` are block scoped, so only accessible to code in the same block or in a direct child block.

## Data types in JS
In JS, a primitive is data that is not an object and has not methods or properties. There are seven primitve datatypes in JS:
1. string
2. number
3. boolean
4. bigint
5. symbol
6. undefined
7. null

Most of the time, a primitive value is represented directly at the lowest level of the language implementation.

All primitives are _immutable_; that is, they cannot be altered. It is important not to confuse a primitive itself with a variable assigned a primitive value. The variable may be reassigned to a new value, but the existing value can not be changed in the ways that objects, arrays, and functions can be altered. The language does not offer utilities to mutate primitive values. For more detail see the [mozilla docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures).

## JS abort controller
This is an interface for creating a controller object, which will allow you to abort one or more web requests made by your application as and when desired. You can create a new `AbortController` object using the [`AbortController()`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController/AbortController "AbortController()") constructor. Communicating with a DOM request is done using an [`AbortSignal`](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal) object.

[`AbortController.abort()`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController/abort)Aborts a DOM request before it has completed. This is able to abort [fetch requests](https://developer.mozilla.org/en-US/docs/Web/API/fetch), consumption of any response bodies, and streams.

This is useful when you want to implement a stop-gap feature, ie: if a request takes more than three seconds to resolve, abort it an show the user a different UI to improve UX.

## The `this` keyword, environments contexts and scope
This topic of the `this` keyword is significantly less important in the context of React and other modern frontend programming, as these frameworks all use a funtional programming style. However OOP with JS is still a foundational topic and is fair game for interview questions. Furthermore, **the related topics of lexical environment and execution context are very relevant to functional programming and are high level topics that will show a good grasp of the fundamentals of the language.**

`this` refers to the current execution context, whether that be the object/method it is called within, or the global execution context when called on a function. To fully understand this we need to discuss and define some more high level terms, <span style="color: cyan; font-weight: bold; font-style: italic;">lexical environment, execution context, call stack and scope.</span>

- **Lexical environment**: JS cares about where you right your code and what surrounds it. What line the code is on, whether it is indented in a block and what other code is in the same environment is the lexical environment of that line of code.
- **Execution context**: Your JS code will consist of multiple lexical environments, each with their own execution context. The execution context denotes the code to be run in a given lexical environment, what variables are available to it and what other code will be involved. This is also where scope comes into the equation. When code from one execution context uses surrounding code from a different lexical environment — like a helper function — it creates a new execution context, and adds it to the call stack.
- **Execution/call stack**: JS manages the order of execution of different execution contexts using a stack data structure. The global execution goes onto the stack first and will be the last to complete execution.
- **Scope**: To put it very simply, scope is where in your code a variable is available. JS initially only had the global scope and the function scope, until es6 introduced the block scope and the `const` and `let` keywords to go with it.
	- **Global scope**: Anything defined here is available anywhere else in the code, ie it is visible in all other scopes. 
	- **Function scope**: Anything declared within a function definition, is NOT visible to any code outside the function. NOTE: this does not apply to arrow functions.
	- **Block scope**: anything defined between curly braces that do not belong to a functions definition.
Scopes can be nested, and any nested scope has access to its parent scope. This is related to closure as discussed above.

