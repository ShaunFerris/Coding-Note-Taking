#typescript 

## What is TypeScript?
It's a language extension that adds a typing system on top of javascript, and ultimately complies down to vanilla JS. This means htat you can write typesafe code that is easier to maintain and test, but is still JS as far as the browser or other runtime env is concerned.

TypeScript offers all of JavaScript’s features, and an additional layer on top of these: It's type system. For example, JavaScript provides language primitives like `string` and `number`, but it doesn’t check that you’ve consistently assigned these. TypeScript does.

This means that your existing working JavaScript code is also TypeScript code. The main benefit of TypeScript is that it can highlight unexpected behavior in your code, lowering the chance of bugs.

## Types by inference
TypeScript knows the JavaScript language and will generate types automatically when it can make an inference about what type a variable will be. This combined with intellisense in the IDE creates a powerfully useful feedback loop for the dev. For example in creating a variable and assigning it to a particular value, TypeScript will use the value as its type, and VsCode will show you the type on hover.

![[Pasted image 20230719125153.png]]

By understanding how JavaScript works, TypeScript can build a type-system that accepts JavaScript code but has types. This offers a type-system without needing to add extra characters to make types explicit in your code. That’s how TypeScript knows that `helloWorld` is a `string` in the above example. Most importantly, the Typescript compiler will scream at you if a function with typed args is given an input of the wrong type.

## Specific topics in typescript
 - [[Typescript - Defining Types With Interfaces]]
 - [[Typescript - Custom Types by Composition]]
 - [[Typescript - The Structural Type System]]
 - [[Typescript - The Declare Keyword]]
 - [[Typescript - Basic Typing in React]]