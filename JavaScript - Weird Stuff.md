#javascript #skills 

The following notes will cover some of the weird quirks of JavaScript that you should be aware of, especially if you are coming in from learning another language first.

## Bitwise not operators for rounding numbers
In JavaScript, the double tilde `~~` is a shorthand for performing a bitwise NOT operation twice. It is often used to convert a non-integer value to an integer by truncating any decimal places.

Here's how it works:
1.  The first tilde (~) performs the bitwise NOT operation on the operand, which flips all the bits and subtracts 1 from the result. For example, ~5 would result in -6.
    
2.  The second tilde (~) performs the bitwise NOT operation again on the result of the first tilde, which flips all the bits back to their original values. For example, `~~` 5 would result in 5.

Here's an example usage of the double tilde operator:
```js
var num = 3.14159; 
var integer = ~~num; 
console.log(integer); // Outputs: 3`
```
In this example, the variable `num` is assigned a non-integer value, and the double tilde operator is used to convert it to an integer by truncating the decimal places. The resulting integer is assigned to the variable `integer`, which is then output to the console.