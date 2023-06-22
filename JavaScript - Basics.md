#javascript #operators #syntax #blockformatting #strings #int

Refer to Javascript as JS from here on in. These JS notes will often refer to the differences between JS and python as python was my first language and as such is my main frame of reference.

JavaScript recognizes seven primitive (immutable) data types: `Boolean`, `Null`, `Undefined`, `Number`, `String`, `Symbol` (new with ES6), and `BigInt` (new with ES2020), and one type for mutable items: `Object`. Note that in JavaScript, arrays are technically a type of object.

Standard variable naming is different in JS than in python. Where python uses snake case for variables and caps for constant variables, JS uses camel case eg: thisIsMyVariable for all variables, and constants are differentiated by decalaration, which python does not have.

All pieces of code that are intended to be executed are ended with a  semi-colon.

Variables must be declared on assignment using the keyword let, eg: `let x = y;`
This is a relatively new convention, they used to be declared with the keyword `var`. You can still use var but variables declared this way will always have global scope, whereas with let, the variable will only have scope in the function/if block/for block where they are declared.

Constant variable are declared with the `const` keyword, where in python they are just given capital names and their constancy is not enforced.

Most datatypes behave roughly the same way as in python, eg strings are still immutable.

## Hello world!
Hello world in JS is basically the same as python, but the print function is replaced with `console.log()`.

## Wierd Quirks
JS was kind of a slapdash language in it's early days, and as a result still retains some quirky behavior, like allowing you to divide by zero, or multiply a string by an empty list. To avoid these you can either be aware of this behavior and dilligently avoid writing code that lets JS attempt wierd things, or you can use TypeScript. Typescript is a JS superset with typechecking and exceptions for things like dividing by zero. For some more specifics, see [[JavaScript - Weird Stuff]].

## Operators in JS
The assignment operator in JS is the same as in python.

== in JS is a loose equality, where types are not checked, eg: `1 == '1'` will return true. === is strict and will type check before checking equality. The inequality operator `!=` is also loose and will convert types in the same way. Strict inequality is `!==`.

Types can be checked with the `typeof` operator, similar to the `type()` function in python.

Modulus division is done using the `%` operator in JS, as opposed to pythons `//` operator.
Multiplication, division, addition, subtraction and exponentiation are the same as python, `*, /, +, -, **` respectively.

Increments of one in JS are done with `++` and decrements of one can be done with `--`. Increments and decrements of more than one can be done with `+= and -=` just like in python. There are also equivalent operators for modulus, exponentiation etc, eg: `x **= y` means x = x to the power of y.

### Booleans in JS
&& is equivalent to the and keyword in python. || is equivalent to the or keyword.

## if/else statement structure in JS
Where python uses only indentation to delimit code blocks, and colons to indicate the start of a block, JS uses curly braces to encapsulate code blocks like if statements. Similar style and structure will be used in while loops, for loops, function definitions etc:
```js
if (num > 10) {
	return "Bigger than 10;"
} else {
	return "10 or less";
}
```
More advanced flow contorl statements can be created using [[JavaScript - The Ternary Operator]].

## ES6 basics
ES6 stands for **ECMAScript 6**. ECMAScript was created to standardize JavaScript, and ES6 is the 6th version of ECMAScript, it was published in 2015, and is also known as ECMAScript 2015.
This section will cover the basic upgrades to structure and sytax that arrived with ES6, more advanded ES6 topics will get their own notes but will be hashtagged with #ES6 .

As previously mentioned, the let keyword was added with ES6, allowing variables to be declared with block scope, rather than defaulting to global scope as when defined with the var keyword.

When you use the const decalration, you are declaring a variable that cannot be reassigned, however const variables are still mutable, so a const list could be modified in place, for example with a push operation. To make a variable immutable and thus fully constant, ES6 added the Object.freeze() function.

## Modules
To make functions or variables from one piece of JS code available to another JS or HTML script, the must be exported from their main script like so:
```js
export {addFunction, subtractFunction};
```
They can then be imported with the syntax:
```js
import {addFunction, subtractFunction} from './mathFunctions.js';
```
To import the entire contents of a file, use the syntax below:
```js
import * as myMathModule from "./math_functions.js";
```

## Math
Some common math operations that are available in python through operators are accessed in JS by methods of the Math prototype. Some examples of commmon operations are shown below, for more advanced uses of Math methods, see the [docs.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

### Math.sqrt
Takes one arg that is a positive number or zero and returns the square root. If a negative number is supplied, the function will retrun NaN.
```js
Math.sqrt(-1); // NaN
Math.sqrt(-0); // -0
Math.sqrt(0); // 0
Math.sqrt(1); // 1
Math.sqrt(2); // 1.414213562373095
Math.sqrt(9); // 3
Math.sqrt(Infinity); // Infinity
```

### Math.PI
This is a static data property of the Math prototype that gives the value of PI to 15 dp.
```js
function calculateCircumference(radius) {
  return Math.PI * (radius + radius);
}

calculateCircumference(1); // 6.283185307179586
```

### Math.round()
The **`Math.round()`** static method returns the value of a number rounded to the nearest integer.
```js
Math.round(-Infinity); // -Infinity
Math.round(-20.51); // -21
Math.round(-20.5); // -20
Math.round(-0.1); // -0
Math.round(0); // 0
Math.round(20.49); // 20
Math.round(20.5); // 21
Math.round(42); // 42
Math.round(Infinity); // Infinity
```

### Math.pow()
The **`Math.pow()`** static method returns the value of a base raised to a power. That is
```js
// Simple cases
Math.pow(7, 2); // 49
Math.pow(7, 3); // 343
Math.pow(2, 10); // 1024

// Fractional exponents
Math.pow(4, 0.5); // 2 (square root of 4)
Math.pow(8, 1 / 3); // 2 (cube root of 8)
Math.pow(2, 0.5); // 1.4142135623730951 (square root of 2)
Math.pow(2, 1 / 3); // 1.2599210498948732 (cube root of 2)

// Signed exponents
Math.pow(7, -2); // 0.02040816326530612 (1/49)
Math.pow(8, -1 / 3); // 0.5

// Signed bases
Math.pow(-7, 2); // 49 (squares are positive)
Math.pow(-7, 3); // -343 (cubes can be negative)
Math.pow(-7, 0.5); // NaN (negative numbers don't have a real square root)
```

## What's next
For more advanced notes on topics in JS:
- [[JavaScript - REGEX]]
- [[JavaScript - Objects]]
- [[JavaScript - Looping]]
- [[JavaScript - Promises]]
- [[JavaScript - Random  Numbers]]
- [[JavaScript - Template Literals]]
- [[JavaScript - Switch statements]]
- [[JavaScript - The Ternary Operator]]
- [[JavaScript - Object Destructuring]]
- [[JavaScript - ES6 Objects and the Class Keyword]]
- [[JavaScript - Immediately Invoked Function Expression]]
- [[JavaScript - The Rest Function and Handling Multiple Function Args]]
- [[JavaScript - Higher Order Array Functions and Functions as First Class Objects]]
- [[Javascript - Async Await]]
- [[Javascript - Callback functions]]