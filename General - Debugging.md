#debugging #python #javascript #skills

Debugging is the process of going through code to figure out and fix the reasons that it is not performing the way you want. The three main types of bug can be thought of as:
1. Syntax errors. These prevent the code from running
2. Runtime errors. These cause unexpected behaviour when the code does run.
3. Logical errors. These cause the code to perform it's task but not as intended.

In this note we will go ever general concepts around debugging. Specific notes about JS and python debuggers can be found here: [[Python - PDB The python debugger]]

## Console debugging
The most basic and useful tool for debugging is printing to the console (using print() in python or console.log() in JS). Adding these statements regurlarly throughout your code allows you to check the value of variables at different points in the code.

It is also useful to check the type of a variable when debugging this way. In JS use `consol.log(typeof variable`. In python use `print(type(variable))`.

## Common syntax errors
Most of these are easily caught by the idea and easily noticeable by paying attention to syntax highlighting but still worth being familiar with.

- Spelling mistakes in variable names - throw a reference error
- Unclosed parentheses
- Mixed usage of single and double quotes for strings
- Missing parentheses for a function call

