#javascript #operators #syntax #blockformatting

Refer to Javascript as JS from here on in. These JS notes will often refer to the differences between JS and python as python was my first language and as such is my main frame of reference.

Standard variable naming is different in JS than in python. Where python uses snake case for variables and caps for constant variables, JS uses camel case eg: thisIsMyVariable for all variables, and constants are differentiated by decalaration, which python does not have.

All pieces of code that are intended to be executed are ended with a  semi-colon.

Variables must be declared on assignment using the keyword let, eg: `let x = y;`
This is a relatively new convention, they used to be declared with the keyword `var`. You can still use var but variables declared this way will always have global scope, whereas with let, the variable will only have scope in the function/if block/for block where they are declared.

Constant variable are declared with the `const` keyword, where in python they are just given capital names and their constancy is not enforced.

Most datatypes behave roughly the same way as in python, eg strings are still immutable.

## Hello world!
Hello world in JS is basically the same as python, but the print function is replaced with `console.log()`.

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
```
if (num > 10) {
	return "Bigger than 10;"
} else {
	return "10 or less";
}
```

## ES6 basics
ES6 stands for **ECMAScript 6**. ECMAScript was created to standardize JavaScript, and ES6 is the 6th version of ECMAScript, it was published in 2015, and is also known as ECMAScript 2015.
This section will cover the basic upgrades to structure and sytax that arrived with ES6, more advanded ES6 topics will get their own notes but will be hashtagged with #ES6 .

As previously mentioned, the let keyword was added with ES6, allowing variables to be declared with block scope, rather than defaulting to global scope as when defined with the var keyword.

When you use the const decalration, you are declaring a variable that cannot be reassigned, however const variables are still mutable, so a const list could be modified in place, for example with a push operation. To make a variable immutable and thus fully constant, ES6 added the Object.freeze() function.