#dsa #datastructures #algorithms #algorithmoptimization #stacks 

The stack is one of the most fundamental data structures in computing and is involved in the running of all computer programs. A stack is a linear data structure that follows a <span style="color: cyan;">last in first out</span> data flow pattern, just like 'the stack' in Magic the Gathering.

A stack data structure has <span style="color: cyan;">four main operations:</span>
1. push() = add an element to the top
2. pop() = remove an element from the top
3. top() = show the element at the top
4. isEmpty() = return a bool that reflects if the stack is empty

The stack is such a fundamental data structure that many computer instruction sets (like assembly) provide special instructions for stack manipulation, and a call stack is the data structure used to handle the execution of programs.

## Stacks and the execution of programs
When we run a program, it is loaded into memory as machine code with instructions stored sequentially. In this view we can look at functions as "mini programs" inside our main program because the code for them is stroed elsewhere in memory. When we say `print(4)` as part of our program, we are essentially saiying "jump to where the code for the print function is, and start sequentially executing instructions until the function is complete, then return to the original location and continue executing the main script where we left off".

The CPU has many special pieces of memory inside it, called registers. Registers are like small pieces of special RAM, usually 32 or 64 bits is size, that perform special functions specific to the CPU. One of theses registers is called the instruction pointer or IP, and it contains the memory address of the next piece of code that the CPU needs to execute.

In the example above, looking at one print statement inside a larger program we might visualise some memory addresses like this:

<table>
  <tr>
    <th>Memory Address</th>
    <th>Instruction</th>
  </tr>
  <tr>
    <td>0x0000001</td>
    <td>Code for the print function</td>
  </tr>
  <tr>
    <td>0x0000002</td>
    <td>Code for the print function</td>
  </tr>
    <tr>
    <td>0x0000003</td>
    <td>Code for the print function</td>
  </tr>
    <tr>
    <td>0x0000004</td>
    <td>Some instruction before the print statement</td>
  </tr>
    <tr>
    <td>0x0000005</td>
    <td>print(4)</td>
  </tr>
    <tr>
    <td>0x0000006</td>
    <td>Some instruction after the print</td>
  </tr>
</table>

We execute the instruction at 0x0000004, then the IP increments to 0x0000005 which is the print statement, but this code is stored at 0x0000001-3. This is where the stack comes into play. We know that when we print a number we want t o continue where we left off and execute the instruction at 0x0000006. In this case we push the **return address** 0x0000006 onto the stack, set the IP to 0x0000001 and start executing the print function code. When it is finished, we pop the return address of the call stack and place it in the IP, continuing the program where it left off.

This makes it easier to visualize recursive processes as essentially a series of push, push, push....pop, pop, pop operations. Things are a lot more complex than this, but it isn't necessary for the  explanation of how stacks are used at the most fundamental levels.

Stacks have many high levels uses too, for example, undo mechanisms in text editors in which we keep track of all text changes in a stack. They may be used in browsers to implement a back/forward mechanism to go back and forward between web pages.

## Building a stack
Below is a simple implementation of a stack in Python, using an array:
```python
class Stack:
	def __init__(self):
		self.stack = []

	def push(self, elem):
		self.stack.append(elem)

	def pop(self):
		if len(self.stack) == 0:
			return None
		else:
			return self.stack.pop()

	def is_empty(self):
		if len(self.stack) ==0:
			return True
		return False
		
	def top(self):
		return self.stack[-1]
```

For an example of a classic application of the stack data structure, see: [[Algorithms - The Matching Brackets Problem]]

