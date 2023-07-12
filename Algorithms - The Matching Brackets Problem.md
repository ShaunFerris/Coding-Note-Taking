#dsa #algorithms #datastructures #stacks

The matching brackets problem is a classic example of where to use stacks, and the application of stacks to this problem will make solving it very straightforward.

The problem is this:
<blockquote style="color: cyan; font-weight: bold; font-style: italic">Given an input string consisting of opening and closing parenteses, write a function that outputs whether the string is balanced</blockquote>
For example: `([{}])` is balanced, but `{()[]` is not.

To approach this problem with a stack, we can take the following angle:
When we encounter a closing bracket, we pop from the stack and check that the element we popped is the opening bracket for the closing one. 

