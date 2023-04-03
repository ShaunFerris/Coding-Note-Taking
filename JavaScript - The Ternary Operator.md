The ternary operator in JS, also called the conditional operator, is the only operator in the language that takes three operands. The format of conditional statements with the operator are of the form:
`(a)condition ? (b)truthy expression : (c)falsy expression;`
- (a) the condition to be checked
- (b) the expression that will be executed if the condition is evaluated to truthy
- (c) the expression that will be executed if the condition is evaluated to falsy

The same functionality can be achieved in python using statements of the format:
`a if condition else b`

Ternary operations are frequently used as an alternative to if...else statements.

## Chaining ternary operations
Ternary operations can be chained together to check multple condition as in the example below. This expression has been formatted across multiple lines for readability, but is technically a single line of code:
```
function findGreaterOrEqual(a, b) {
	return (a === b) ? "a and b are equal"
		: (a > b) "a is greater"
		: "b is greater";
}
```
This statement can be read as a direct replacement to the format: if a is equal to b do this, else if a is greater than b do this, else do this.