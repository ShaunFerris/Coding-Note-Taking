#javascript #operators #syntax #blockformatting

Refer to Javascript as JS from here on in. These JS notes will often refer to the differences between JS and python as python was my first language and as such is my main frame of reference.

All pieces of code that are intended to be executed are ended with a ;

Variables must be declared on assignment using the keyword let, eg: `let x = y;`
This is a relatively new convention, they used to be declared with the keyword var. You can still use var but variables declared this way will always have global scope, whereas with let, the variable will only have scope in the function/if block/for block where they are declared.

Constant variable are declared with the const keyword, where in python they are just given capital names and their constancy is not enforced.

Most datatypes behave roughly the same way as in python, eg strings are still immutable.

## Operators in JS
== in JS is a loose equality, where types are not checked, eg: `1 == '1'` will return true. === is strict and will type check before checking equality. The inequality operator `!=` is also loose and will convert types in the same way. Strict inequality is `!==`.

Types can be checked with the `typeof` operator, similar to the `type()` function in python.

### Booleans in JS
&& is equivalent to the and keyword in python. || is equivalent to the or keyword.

## if/else statement structure in JS
Where python uses only indentation to delimit code blocks, JS uses curly braces. Similar style and structure will be used in while loops, for loops, function definitions etc:
```
if (num > 10) {
	return "Bigger than 10;"
} else {
	return "10 or less";
}
```
