#paradigms #highlevelconcepts #algorithms #functions 

Functional programming is a style of programming where solutions are simple, isolated functions, without any side effects outside of the function scope: `INPUT -> PROCESS -> OUTPUT`

Functional programming is about:
1.  Isolated functions - there is no dependence on the state of the program, which includes global variables that are subject to change
2.  Pure functions - the same input always gives the same output
3.  Functions with limited side effects - any changes, or mutations, to the state of the program outside the function are carefully controlled

## Functional programming terminology
Callbacks are the functions that are slipped or passed into another function to decide the invocation of that function. You may have seen them passed to other methods, for example in `filter`, the callback function tells JavaScript the criteria for how to filter an array.

Functions that can be assigned to a variable, passed into another function, or returned from another function just like any other normal value, are called first class functions. In JavaScript, all functions are first class functions.

The functions that take a function as an argument, or return a function as a return value, are called higher order functions.

When functions are passed in to or returned from another function, then those functions which were passed in or returned can be called a lambda.

## Functional programming in practice
One of the core principles of functional programming is to not change things. Changes lead to bugs. It's easier to prevent bugs knowing that your functions don't change anything, including the function arguments or any global variable.

Another principle of functional programming is to always declare your dependencies explicitly. This means if a function depends on a variable or object being present, then pass that variable or object directly into the function as an argument.

Some solid concepts to be aware of when thinkning about functional programming:
1.  Don't alter a variable or object - create new variables and objects and return them if need be from a function. Hint: using something like `const newArr = arrVar`, where `arrVar` is an array will simply create a reference to the existing variable and not a copy. So changing a value in `newArr` would change the value in `arrVar`.
    
2.  Declare function parameters - any computation inside a function depends only on the arguments passed to the function, and not on any global object or variable.

For a more in depth look at functional programming in practice with specific reference to JS, see [[JavaScript - Higher Order Functions and Functions as First Class Objects]].

For higher order functions in python see [[Python - Map, Filter and Reduce Functions]].